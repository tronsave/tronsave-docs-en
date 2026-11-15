---
description: A complete, runnable example that buys Energy or Bandwidth with an API key and polls the order until it is fulfilled, in JavaScript, Python, and PHP.
---

# Buy Resources with an API Key

This is a full working example that buys resources (Energy or Bandwidth) using an [API key](../../authentication.md). It checks the order book and your internal account balance, places a buy order, then polls the order until it is fulfilled.

The flow calls these v2 endpoints:

* `GET /v2/order-book` — see available liquidity and prices.
* `GET /v2/user-info` — check your internal account balance.
* `POST /v2/buy-resource` — place the order and receive an `orderId`.
* `GET /v2/order/{orderId}` — poll the order until `fulfilledPercent` reaches 100.

## Before you start

You need an API key. Generate one in two ways:

* On the TronSave website. See [Authentication](../../authentication.md).
* On Telegram. See [Authentication](../../authentication.md).

{% hint style="warning" %}
Your API key spends from your internal account balance. Never commit it to source control or expose it in client-side code. See [Authentication](../../authentication.md).
{% endhint %}

### Configuration values

Replace the placeholder values at the top of each example before running:

| Constant | Meaning |
| --- | --- |
| `API_KEY` | Your TronSave API key. |
| `TRONSAVE_API_URL` | API base URL. Use `https://api.tronsave.io` for mainnet or `https://api-dev.tronsave.io` for testnet. See [Environments](../../environments.md). |
| `RECEIVER_ADDRESS` | The address that receives the delegated resources. |
| `BUY_AMOUNT` | Amount of resource to buy (e.g. `32000`). |
| `DURATION` | Order duration in seconds (`3600` = 1 hour). |
| `MAX_PRICE_ACCEPTED` | Maximum price (in SUN per unit) you are willing to pay. |
| `RESOURCE_TYPE` | `ENERGY` or `BANDWIDTH`. |

{% hint style="info" %}
The JavaScript example requires **TronWeb 5.3.2**:

```bash
npm i tronweb@5.3.2 @noble/secp256k1@1.7.1
```

Read more in the [TronWeb 5.3.2 release notes](https://tronweb.network/docu/docs/5.3.2/Release%20Note/).
{% endhint %}

## Full example

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const API_KEY = `your_api_key`; // change it
const TRONSAVE_API_URL = "https://api-dev.tronsave.io" //in testnet mode change it to "https://api-dev.tronsave.io"
const RECEIVER_ADDRESS = 'your_receiver_address' // change it
const BUY_AMOUNT = 32000 // change it
const DURATION = 3600 // current value: 1h. change it
const MAX_PRICE_ACCEPTED = 100 // change it
const RESOURCE_TYPE = "ENERGY" // ENERGY or BANDWIDTH

const sleep = async (ms) => {
    await new Promise((resolver, reject) => {
        setTimeout(() => resolver("OK"), ms)
    })
}

const GetOrderBook = async (apiKey, receiverAddress) => {
    const url = `${TRONSAVE_API_URL}/v2/order-book?address=${receiverAddress}`
    const data = await fetch(url, {
        headers: {
            'apikey': apiKey
        }
    })
    const response = await data.json()
    /**
     * Example response 
    {
        error: false,
        message: 'Success',
        data: [
            { price: 54, availableResourceAmount: 2403704 },
            { price: 60, availableResourceAmount: 3438832 },
            { price: 61, availableResourceAmount: 4100301 },
            { price: 90, availableResourceAmount: 7082046 },
            { price: 91, availableResourceAmount: 7911978 }
        ]
    }
     */
    return response
}

const GetAccountInfo = async (apiKey) => {
    const url = `${TRONSAVE_API_URL}/v2/user-info`
    const data = await fetch(url, {
        headers: {
            'apikey': apiKey
        }
    })
    const response = await data.json()
    /**
     * Example response 
    {
        "error": false,
        "message": "Success",
        "data": {
            "id": "67a2e6092...2e8b291da2",
            "balance": "373040535",
            "representAddress": "TTgMEAhuzPch...nm2tCNmqXp13AxzAd",
            "depositAddress": "TTgMEAhuzPch...2tCNmqXp13AxzAd"
        }
    }
     */
    return response
}


const BuyResource = async (apiKey, receiverAddress, resourceAmount, durationSec, maxPriceAccepted) => {
    const url = `${TRONSAVE_API_URL}/v2/buy-resource`
    const body = {
        resourceType: RESOURCE_TYPE,
        unitPrice: "MEDIUM", //price in sun or "SLOW"|"MEDIUM"|"FAST"
        resourceAmount, //Amount of resource want to buy
        receiver: receiverAddress,
        durationSec, //order duration in sec. Default: 259200 (3 days)
        options: {
            allowPartialFill: true,
            onlyCreateWhenFulfilled: false,
            maxPriceAccepted,
        }
    }
    const data = await fetch(url, {
        method: "POST",
        headers: {
            'apikey': apiKey,
            "content-type": "application/json",
        },
        body: JSON.stringify(body)
    })
    const response = await data.json()
    /**
     * Example response
     * {
     *     "error": false,
     *     "message": "Success",
     *     "data": {
     *         "orderId": "6809fdb7b9...a41d726fd"
     *     }
     * }
     */
    return response
}

const GetOneOrderDetails = async (api_key, order_id) => {
    const url = `${TRONSAVE_API_URL}/v2/order/${order_id}`
    const data = await fetch(url, {
        headers: {
            'apikey': api_key
        }
    })
    const response = await data.json()
    /**
     * Example response 
        {
            "error": false,
            "message": "Success",
            "data": {
                "id": "680b3e9939...600b7734d",
                "requester": "TTgMEAhuzPch...nm2tCNmqXp13AxzAd",
                "receiver": "TAk6jzZqHwNU...yAE1YAoUPk7r2T6h",
                "resourceAmount": 32000,
                "resourceType": "ENERGY",
                "remainAmount": 0,
                "price": 91,
                "durationSec": 3600,
                "orderType": "NORMAL",
                "allowPartialFill": false,
                "payoutAmount": 2912000,
                "fulfilledPercent": 100,
                "delegates": [
                    {
                        "delegator": "TQ5VcQjA7w...Pio485UDhCWAANrMh",
                        "amount": 32000,
                        "txid": "b200e8b7f9130b67ff....403c51d6f7a92acc7c4618906c375b69"
                    }
                ]
            }
        }
     */
    return response
}
const GetOrderHistory = async (apiKey) => {
    const url = `${TRONSAVE_API_URL}/v2/orders`
    const data = await fetch(url, {
        headers: {
            'apikey': apiKey
        }
    })
    const response = await data.json()
     /**
     * Example response 
        {
            "error": false,
            "message": "Success",
            "data": 
            {
                "total": 2,
                "data": [
                    {
                        "id": "6809b08a14b1cb7c5d195d66",
                        "requester": "TTgMEAhuzPchDAL4pnm2tCNmqXp13AxzAd",
                        "receiver": "TFwUFWr3QV376677Z8VWXxGUAMFSrq1MbM",
                        "resourceAmount": 40000,
                        "resourceType": "ENERGY",
                        "remainAmount": 0,
                        "orderType": "NORMAL",
                        "price": 81.5,
                        "durationSec": 900,
                        "allowPartialFill": false,
                        "payoutAmount": 3260000,
                        "fulfilledPercent": 100,
                        "delegates": [
                            {
                                "delegator": "THnnMCe67VMDXoivepiA7ZQSB8jbgKDodf",
                                "amount": 40000,
                                "txid": "19be98d0183b29575d74999a93154b09b3c7d05051cdbd52c667cd9f0b3cc9b0"
                            }
                        ]
                    },
                    {
                        "id": "6809aaf2e2e17d3c588b467a",
                        "requester": "TTgMEAhuzPchDAL4pnm2tCNmqXp13AxzAd",
                        "receiver": "TFwUFWr3QV376677Z8VWXxGUAMFSrq1MbM",
                        "resourceAmount": 40000,
                        "resourceType": "ENERGY",
                        "remainAmount": 0,
                        "orderType": "NORMAL",
                        "price": 81.5,
                        "durationSec": 900,
                        "allowPartialFill": false,
                        "payoutAmount": 3260000,
                        "fulfilledPercent": 100,
                        "delegates": [
                            {
                                "delegator": "THnnMCe67VMDXoivepiA7ZQSB8jbgKDodf",
                                "amount": 40000,
                                "txid": "447e3fb28ad7580554642d08b9a6b220bc86f667b47edad47f16802594b6b1e3"
                            }
                        ]
                    },
                ]
            }
        }
     */
    return response
}
const CreateOrderByUsingApiKey = async () => {
    //Check energy available
    const orderBook = await GetOrderBook(API_KEY, RECEIVER_ADDRESS)
    console.log(orderBook)
    /*
      {
        error: false,
        message: 'Success',
        data: [
            { price: 54, availableResourceAmount: 2403704 },
            { price: 60, availableResourceAmount: 3438832 },
            { price: 90, availableResourceAmount: 6420577 },
            { price: 91, availableResourceAmount: 7250509 }
        ]
    }
    */
    //Look at response above, we have 177k energy at price less than 30, 331k enegy at price 30 and 2841k energy at price 35
    //Example if want to buy 500k energy in 3 days you have to place order at price at least 35 energy to fulfill your order (the price can be higher if duration of order less than 3 days)

    const needTrx = MAX_PRICE_ACCEPTED * BUY_AMOUNT

    //Check if your internal balance enough to buy
    const accountInfo = await GetAccountInfo(API_KEY)
    console.log(accountInfo)
    /*
     {
        error: false,
        message: 'Success',
        data: {
            id: '67a2e609....e8b291da2',
            balance: '370352535',
            representAddress: 'TTgMEAhuzPc....m2tCNmqXp13AxzAd',
            depositAddress: 'TTgMEAhuzPch....CNmqXp13AxzAd'
        }
    }
    */
    const isBalanceEnough = Number(accountInfo.data.balance) >= needTrx
    console.log({ isBalanceEnough })
    if (isBalanceEnough) {
        const buyResourceOrder = await BuyResource(API_KEY, RECEIVER_ADDRESS, BUY_AMOUNT, DURATION, MAX_PRICE_ACCEPTED)
        console.log(buyResourceOrder)
        /*
          {
            error: false,
            message: 'Success',
            data: { orderId: '680b3e993...600b7734d' }
        }
        */
        //Wait 3-5 seconds after buy then check 
        if (!buyResourceOrder.error) {
            while (true) {
                await sleep(3000)
                const orderDetail = await GetOneOrderDetails(API_KEY, buyResourceOrder.data.orderId)
                console.log(orderDetail)
                /*
                    {
                        "error": false,
                        "message": "Success",
                        "data": {
                            "id": "680b3e9....3600b7734d",
                            "requester": "TTgMEAhuzPchDAL....CNmqXp13AxzAd",
                            "receiver": "TAk6jzZqHwNU...oUPk7r2T6h",
                            "resourceAmount": 32000,
                            "resourceType": "ENERGY",
                            "remainAmount": 0,
                            "price": 91,
                            "durationSec": 3600,
                            "orderType": "NORMAL",
                            "allowPartialFill": false,
                            "payoutAmount": 2912000,
                            "fulfilledPercent": 100,
                            "delegates": [
                                {
                                    "delegator": "TQ5VcQjA7wkUJ7....85UDhCWAANrMh",
                                    "amount": 32000,
                                    "txid": "b200e8b7f9130b67ff29e5e2....d6f7a92acc7c4618906c375b69"
                                }
                            ]
                        }
                    }
                */
                if (orderDetail && orderDetail.data.fulfilledPercent === 100 || orderDetail.data.remainAmount === 0) {
                    console.log(`Your order already fulfilled`)
                    break;
                } else {
                    console.log(`Your order is not fulfilled, wait 3s and recheck`)
                }
            }
        } else {
            console.log({ buyResourceOrder })
            throw new Error(`Buy Order Failed`)
        }
    }
}

CreateOrderByUsingApiKey()
```
{% endtab %}

{% tab title="Python" %}
```python
import time
import requests
from typing import Dict, List, Any, Optional

# Configuration
API_KEY = 'your-api-key'
TRONSAVE_API_URL = "https://api.tronsave.io"
RECEIVER_ADDRESS = 'your-receiver-address'
BUY_AMOUNT = 32000
DURATION = 3600  # 1 hour
MAX_PRICE_ACCEPTED = 100
RESOURCE_TYPE = "ENERGY"  # ENERGY or BANDWIDTH

def sleep_ms(ms: int) -> None:
    """Sleep for specified milliseconds"""
    time.sleep(ms / 1000)

def get_order_book() -> Dict[str, Any]:
    """Get order book for resources"""
    url = f"{TRONSAVE_API_URL}/v2/order-book"
    headers = {'apikey': API_KEY}
    params = {'address': RECEIVER_ADDRESS}
    
    response = requests.get(url, headers=headers, params=params)
    return response.json()

def get_account_info() -> Dict[str, Any]:
    """Get account information"""
    url = f"{TRONSAVE_API_URL}/v2/user-info"
    headers = {'apikey': API_KEY}
    
    response = requests.get(url, headers=headers)
    return response.json()

def buy_resource(amount: int, duration_sec: int, max_price_accepted: int) -> Dict[str, Any]:
    """Buy resource"""
    url = f"{TRONSAVE_API_URL}/v2/buy-resource"
    headers = {
        'apikey': API_KEY,
        'Content-Type': 'application/json'
    }
    
    body = {
        'resourceType': RESOURCE_TYPE,
        'unitPrice': "MEDIUM",
        'amount': amount,
        'receiver': RECEIVER_ADDRESS,
        'durationSec': duration_sec,
        'options': {
            'allowPartialFill': True,
            'onlyCreateWhenFulfilled': False,
            'maxPriceAccepted': max_price_accepted,
        }
    }
    
    response = requests.post(url, headers=headers, json=body)
    return response.json()

def get_order_details(order_id: str) -> Dict[str, Any]:
    """Get order details"""
    url = f"{TRONSAVE_API_URL}/v2/order/{order_id}"
    headers = {'apikey': API_KEY}
    
    response = requests.get(url, headers=headers)
    return response.json()

def create_order_by_using_api_key() -> None:
    """Main function to create order"""
    try:
        # Check energy available
        order_book = get_order_book()
        print("Order Book:", order_book)

        need_trx = MAX_PRICE_ACCEPTED * BUY_AMOUNT

        # Check if balance is enough
        account_info = get_account_info()
        print("Account Info:", account_info)

        is_balance_enough = int(account_info['data']['balance']) >= need_trx
        print(f"Is balance enough: {is_balance_enough}")

        if is_balance_enough:
            buy_resource_order = buy_resource(BUY_AMOUNT, DURATION, MAX_PRICE_ACCEPTED)
            print("Buy Resource Order:", buy_resource_order)

            if not buy_resource_order['error']:
                while True:
                    sleep_ms(3000)
                    order_detail = get_order_details(buy_resource_order['data']['orderId'])
                    print("Order Detail:", order_detail)

                    if (order_detail['data']['fulfilledPercent'] == 100 or 
                        order_detail['data']['remainAmount'] == 0):
                        print("Order fulfilled successfully")
                        break
                    else:
                        print("Order not fulfilled, waiting 3s and rechecking...")
            else:
                print("Buy Resource Order:", buy_resource_order)
                raise Exception("Buy Order Failed")

    except Exception as e:
        print(f"Error: {str(e)}")

if __name__ == "__main__":
    create_order_by_using_api_key() 

```
{% endtab %}

{% tab title="PHP" %}
```php
<?php

// Configuration
const API_KEY = 'your_api_key';
const TRONSAVE_API_URL = "https://api-dev.tronsave.io";
const RECEIVER_ADDRESS = 'your_receiver_address';
const BUY_AMOUNT = 32000;
const DURATION = 3600; // 1 hour
const MAX_PRICE_ACCEPTED = 100;
const RESOURCE_TYPE = "ENERGY";

function sleep_ms($ms) {
    usleep($ms * 1000);
}

function getOrderBook($apiKey, $receiverAddress) {
    $url = TRONSAVE_API_URL . "/v2/order-book?address=" . $receiverAddress;
    $ch = curl_init();
    curl_setopt_array($ch, [
        CURLOPT_URL => $url,
        CURLOPT_RETURNTRANSFER => true,
        CURLOPT_HTTPHEADER => ['apikey: ' . $apiKey]
    ]);
    $response = curl_exec($ch);
    curl_close($ch);
    return json_decode($response, true);
}

function getAccountInfo($apiKey) {
    $url = TRONSAVE_API_URL . "/v2/user-info";
    $ch = curl_init();
    curl_setopt_array($ch, [
        CURLOPT_URL => $url,
        CURLOPT_RETURNTRANSFER => true,
        CURLOPT_HTTPHEADER => ['apikey: ' . $apiKey]
    ]);
    $response = curl_exec($ch);
    curl_close($ch);
    return json_decode($response, true);
}

function buyResource($apiKey, $receiverAddress, $resourceAmount, $durationSec, $maxPriceAccepted) {
    $url = TRONSAVE_API_URL . "/v2/buy-resource";
    $body = [
        'resourceType' => RESOURCE_TYPE,
        'unitPrice' => "MEDIUM",
        'resourceAmount' => $resourceAmount,
        'receiver' => $receiverAddress,
        'durationSec' => $durationSec,
        'options' => [
            'allowPartialFill' => true,
            'onlyCreateWhenFulfilled' => false,
            'maxPriceAccepted' => $maxPriceAccepted,
        ]
    ];

    $ch = curl_init();
    curl_setopt_array($ch, [
        CURLOPT_URL => $url,
        CURLOPT_POST => true,
        CURLOPT_POSTFIELDS => json_encode($body),
        CURLOPT_RETURNTRANSFER => true,
        CURLOPT_HTTPHEADER => [
            'apikey: ' . $apiKey,
            'Content-Type: application/json'
        ]
    ]);
    $response = curl_exec($ch);
    curl_close($ch);
    return json_decode($response, true);
}

function getOneOrderDetails($apiKey, $orderId) {
    $url = TRONSAVE_API_URL . "/v2/order/" . $orderId;
    $ch = curl_init();
    curl_setopt_array($ch, [
        CURLOPT_URL => $url,
        CURLOPT_RETURNTRANSFER => true,
        CURLOPT_HTTPHEADER => ['apikey: ' . $apiKey]
    ]);
    $response = curl_exec($ch);
    curl_close($ch);
    return json_decode($response, true);
}

function createOrderByUsingApiKey() {
    try {
        // Check energy available
        $orderBook = getOrderBook(API_KEY, RECEIVER_ADDRESS);
        print_r($orderBook);

        $needTrx = MAX_PRICE_ACCEPTED * BUY_AMOUNT;

        // Check if balance is enough
        $accountInfo = getAccountInfo(API_KEY);
        print_r($accountInfo);

        $isBalanceEnough = (int)$accountInfo['data']['balance'] >= $needTrx;
        echo "Is balance enough: " . ($isBalanceEnough ? "Yes" : "No") . "\n";

        if ($isBalanceEnough) {
            $buyResourceOrder = buyResource(API_KEY, RECEIVER_ADDRESS, BUY_AMOUNT, DURATION, MAX_PRICE_ACCEPTED);
            print_r($buyResourceOrder);

            if (!$buyResourceOrder['error']) {
                while (true) {
                    sleep_ms(3000);
                    $orderDetail = getOneOrderDetails(API_KEY, $buyResourceOrder['data']['orderId']);
                    print_r($orderDetail);

                    if ($orderDetail['data']['fulfilledPercent'] === 100 || $orderDetail['data']['remainAmount'] === 0) {
                        echo "Your order already fulfilled\n";
                        break;
                    } else {
                        echo "Your order is not fulfilled, wait 3s and recheck\n";
                    }
                }
            } else {
                print_r($buyResourceOrder);
                throw new Exception("Buy Order Failed");
            }
        }
    } catch (Exception $e) {
        echo "Error: " . $e->getMessage() . "\n";
    }
}

// Run the main function
createOrderByUsingApiKey();

```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
`unitPrice` accepts either an explicit price in SUN or one of the presets `SLOW`, `MEDIUM`, or `FAST`. The order is created against the order book, so set `MAX_PRICE_ACCEPTED` high enough to cover the liquidity you need — the returned `order-book` data shows the prices at which resources are available.
{% endhint %}

## Next steps

* [Authentication](../../authentication.md) — how API keys and internal accounts work.
* [Create Order (API key)](../../api-reference/buy-resources/api-key/create-order.md) — full reference for `POST /v2/buy-resource`.
* [Get Order Details](../../api-reference/buy-resources/api-key/get-order-details.md) and [Get Order Book](../../api-reference/buy-resources/api-key/get-order-book.md).
* [Order types](../../../concepts/order-types.md) — understand partial fill and fulfillment behavior.
