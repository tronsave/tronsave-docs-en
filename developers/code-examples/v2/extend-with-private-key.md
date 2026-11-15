---
description: A complete working example that extends an existing resource delegation by paying for it with a locally signed transaction built from your private key.
---

# Extend an Order Using a Private Key

This is a full, runnable example showing how to extend an existing resource delegation using the v2 Extend Orders API while paying with a transaction signed locally from your private key.

The flow has two API calls:

1. **`POST /v2/get-extendable-delegates`** — estimates which delegates can be extended and how much TRX it will cost (`totalEstimateTrx`).
2. **`POST /v2/extend-request`** — submits the extend request together with a `signedTx` that pays TronSave the estimated amount.

Between those two calls you build and sign a TRX transfer to TronSave's receiver address using your own private key, so TronSave never holds your funds.

{% hint style="info" %}
This example uses TronWeb to build and sign the transfer. **TronWeb version 5.3.2** is required.

```bash
npm i tronweb@5.3.2 @noble/secp256k1@1.7.1
```

Read more: [https://tronweb.network/docu/docs/5.3.2/Release%20Note/](https://tronweb.network/docu/docs/5.3.2/Release%20Note/)
{% endhint %}

## Configuration

Before running, set the following values:

<table><thead><tr><th width="260">Constant</th><th>Description</th></tr></thead><tbody><tr><td><code>TRONSAVE_API_URL</code></td><td>TronSave API base URL. Mainnet: <code>https://api.tronsave.io</code>.</td></tr><tr><td><code>RECEIVER</code></td><td>The address that receives the extended resource delegation.</td></tr><tr><td><code>RESOURCE_TYPE</code></td><td><code>ENERGY</code> or <code>BANDWIDTH</code>. Optional — defaults to <code>ENERGY</code>.</td></tr><tr><td><code>REQUESTER_ADDRESS</code></td><td>The address that requests (and pays for) the extension.</td></tr><tr><td><code>PRIVATE_KEY</code></td><td>Private key for <code>REQUESTER_ADDRESS</code>, used to sign the TRX transfer locally.</td></tr><tr><td><code>TRON_FULL_NODE</code></td><td>TRON full node used by TronWeb to build the transaction (e.g. <code>https://api.trongrid.io</code>).</td></tr><tr><td><code>TRONSAVE_RECEIVER_ADDRESS</code></td><td>The TronSave address that receives your payment. Mainnet: <code>TWZEhq5JuUVvGtutNgnRBATbF8BnHGyn4S</code>. Testnet: <code>TATT1UzHRikft98bRFqApFTsaSw73ycfoS</code>.</td></tr></tbody></table>

{% hint style="warning" %}
Keep your private key secret. Run this code only in a trusted server environment — never expose `PRIVATE_KEY` in client-side or browser code.
{% endhint %}

See [Environments](../../environments.md) for the full list of mainnet and testnet endpoints, and the [Extend Orders API reference](../../api-reference/extend-orders/README.md) for endpoint details.

## Full example

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const TronWeb = require('tronweb')

const TRONSAVE_API_URL = "https://api.tronsave.io"
const RECEIVER = "your-receiver-address"
const RESOURCE_TYPE = "ENERGY"
const REQUESTER_ADDRESS = "TFwUFWr3QV376677Z8VWXxGUAMFSrq1MbM"
const PRIVATE_KEY = "your-private-key"
const TRON_FULL_NODE = "https://api.trongrid.io"
const TRONSAVE_RECEIVER_ADDRESS = "TWZEhq5JuUVvGtutNgnRBATbF8BnHGyn4S"; //in testnet mode change it to "TATT1UzHRikft98bRFqApFTsaSw73ycfoS"


// Initialize TronWeb instance
const tronWeb = new TronWeb({
    fullNode: TRON_FULL_NODE,
    solidityNode: TRON_FULL_NODE,
    eventServer: TRON_FULL_NODE,
});

const GetEstimateExtendData = async (requester, extendTo, maxPriceAccepted) => {
    const url = TRONSAVE_API_URL + `/v2/get-extendable-delegates`
    const body = {
        extendTo, //time in seconds you want to extend to
        receiver: RECEIVER,   //the address that receives the resource delegate
        requester, //the address that requests the resource delegate
        maxPriceAccepted, //Optional. Number the maximum price you want to pay to extend
        resourceType: RESOURCE_TYPE, //ENERGY or BANDWIDTH. optional. The default is ENERGY
    }
    const data = await fetch(url, {
        method: "POST",
        headers: {
            "content-type": "application/json",
        },
        body: JSON.stringify(body)
    })
    const response = await data.json()
    /**
     * Example response 
     * @link  
       {
            "error": false,
            "message": "Success",
            "data": {
                "extendOrderBook": [
                    {
                        "price": 784,
                        "value": 1002
                    }
                ],
                "totalDelegateAmount": 5003,
                "totalAvailableExtendAmount": 5003,
                "totalEstimateTrx": 4085783,
                "isAbleToExtend": true,
                "yourBalance": 2377366851,
                "extendData": [
                    {
                        "delegator": "TGGVrYaT8Xoos...6dmSZkohGGcouYL4",
                        "isExtend": true,
                        "extraAmount": 0,
                        "extendTo": 1745833276
                    },
                    {
                        "delegator": "TQBV7xU489Rq8Z...zBhJMdrDr51wA2",
                        "isExtend": true,
                        "extraAmount": 0,
                        "extendTo": 1745833276
                    },
                    {
                        "delegator": "TSHZv6xsYHMRCbdVh...qNozxaPPjDR6",
                        "isExtend": true,
                        "extraAmount": 0,
                        "extendTo": 1745833276
                    }
                ]
            }
        }
     */
    return response
}


const GetSignedTx = async (totalEstimateTrx) => {
    const dataSendTrx = await tronWeb.transactionBuilder.sendTrx(TRONSAVE_RECEIVER_ADDRESS, totalEstimateTrx, REQUESTER_ADDRESS);
    const signedTx = await tronWeb.trx.sign(dataSendTrx, PRIVATE_KEY);
    return signedTx;
};
/**
 * @param {number} extendTo time in seconds you want to extend to
 * @param {boolean} maxPriceAccepted number maximum price you want to pay to extend
 * @returns 
 */
const SendExtendRequest = async (extendTo, maxPriceAccepted) => {
    const url = TRONSAVE_API_URL + `/v2/extend-request`
    // Get estimate extendable delegates
    const estimateResponse = await GetEstimateExtendData(REQUESTER_ADDRESS, extendTo, maxPriceAccepted)
    const extendData = estimateResponse.data?.extendData
    // check if there are extendable delegates
    if (extendData && extendData.length) { 
        const totalEstimateTrx = estimateResponse.data?.totalEstimateTrx
        // Build a signed transaction by using the private key
        const signedTx = await GetSignedTx(totalEstimateTrx)
        const body = {
            extendData: extendData,
            receiver: RECEIVER,
            signedTx
        }
        const data = await fetch(url, {
            method: "POST",
            headers: {
                "content-type": "application/json",
            },
            body: JSON.stringify(body)
        })
        const response = await data.json()
        /**
         * Example response 
         {
            error: false,
            message: 'Success',
            data: { orderId: '680b5ac7b09a385fb3d582ff' }
            }
         */
        return response
    }
    return []
}

//Example run code
const ClientCode = async () => { 
    const extendTo = Math.floor(new Date().getTime() / 1000) + 3 * 86400 //Extend to 3 next days
    const maxPriceAccepted = 900
    const response = await SendExtendRequest(extendTo, maxPriceAccepted)
    console.log(response)
}


ClientCode()
```
{% endtab %}

{% tab title="PHP" %}
```php
<?php

// Configuration
const TRONSAVE_API_URL = "https://api.tronsave.io";
const RECEIVER = "your-receiver-address";
const RESOURCE_TYPE = "ENERGY";
const REQUESTER_ADDRESS = "TFwUFWr3QV376677Z8VWXxGUAMFSrq1MbM";
const PRIVATE_KEY = "your-private-key";
const TRON_FULL_NODE = "https://api.trongrid.io";
const TRONSAVE_RECEIVER_ADDRESS = "TWZEhq5JuUVvGtutNgnRBATbF8BnHGyn4S";

/**
 * Get estimate extend data
 * @param string $requester
 * @param int $extendTo
 * @param int $maxPriceAccepted
 * @return array
 */
function getEstimateExtendData(string $requester, int $extendTo, int $maxPriceAccepted): array {
    $url = TRONSAVE_API_URL . "/v2/get-extendable-delegates";
    $body = [
        'extendTo' => $extendTo,
        'receiver' => RECEIVER,
        'requester' => $requester,
        'maxPriceAccepted' => $maxPriceAccepted,
        'resourceType' => RESOURCE_TYPE,
    ];

    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, $url);
    curl_setopt($ch, CURLOPT_POST, true);
    curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($body));
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_HTTPHEADER, [
        'Content-Type: application/json'
    ]);
    $response = curl_exec($ch);
    curl_close($ch);
    return json_decode($response, true);
}

/**
 * Get signed transaction
 * @param int $totalEstimateTrx
 * @return array
 */
function getSignedTx(int $totalEstimateTrx): array {
    // This is a placeholder. In real implementation, you would use TronWeb PHP SDK
    return [
        'visible' => false,
        'txID' => '...',
        'raw_data' => [
            'contract' => [
                [
                    'parameter' => [
                        'value' => [
                            'amount' => $totalEstimateTrx,
                            'owner_address' => REQUESTER_ADDRESS,
                            'to_address' => TRONSAVE_RECEIVER_ADDRESS
                        ],
                        'type_url' => 'type.googleapis.com/protocol.TransferContract'
                    ],
                    'type' => 'TransferContract'
                ]
            ]
        ]
    ];
}

/**
 * Send extend request
 * @param int $extendTo
 * @param int $maxPriceAccepted
 * @return array
 */
function sendExtendRequest(int $extendTo, int $maxPriceAccepted): array {
    $url = TRONSAVE_API_URL . "/v2/extend-request";
    $estimateResponse = getEstimateExtendData(REQUESTER_ADDRESS, $extendTo, $maxPriceAccepted);
    $extendData = $estimateResponse['data']['extendData'] ?? [];

    if (!empty($extendData)) {
        $totalEstimateTrx = $estimateResponse['data']['totalEstimateTrx'];
        $signedTx = getSignedTx($totalEstimateTrx);

        $body = [
            'extendData' => $extendData,
            'receiver' => RECEIVER,
            'signedTx' => $signedTx
        ];

        $ch = curl_init();
        curl_setopt($ch, CURLOPT_URL, $url);
        curl_setopt($ch, CURLOPT_POST, true);
        curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($body));
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
        curl_setopt($ch, CURLOPT_HTTPHEADER, [
            'Content-Type: application/json'
        ]);
        $response = curl_exec($ch);
        curl_close($ch);
        return json_decode($response, true);
    }

    return [];
}

// Example run code
try {
    $extendTo = time() + (3 * 86400); // Extend to 3 next days
    $maxPriceAccepted = 900;
    $response = sendExtendRequest($extendTo, $maxPriceAccepted);
    print_r($response);
} catch (Exception $e) {
    echo "Error: " . $e->getMessage() . "\n";
} 
```

{% hint style="warning" %}
The PHP `getSignedTx()` function above is a placeholder. PHP has no official TronWeb SDK, so you must build and sign the TRX transfer with a TRON-compatible signing library before sending the extend request.
{% endhint %}
{% endtab %}

{% tab title="Python" %}
```python
//import time
import requests
from typing import Dict, Any, Optional

# Configuration
TRONSAVE_API_URL = "https://api.tronsave.io"
RECEIVER = "your-receiver-address"
RESOURCE_TYPE = "ENERGY"
REQUESTER_ADDRESS = "TFwUFWr3QV376677Z8VWXxGUAMFSrq1MbM"
PRIVATE_KEY = "your-private-key"
TRON_FULL_NODE = "https://api.trongrid.io"
TRONSAVE_RECEIVER_ADDRESS = "TWZEhq5JuUVvGtutNgnRBATbF8BnHGyn4S"

def get_estimate_extend_data(requester: str, extend_to: int, max_price_accepted: int) -> Dict[str, Any]:
    """Get estimate extend data"""
    url = f"{TRONSAVE_API_URL}/v2/get-extendable-delegates"
    body = {
        'extendTo': extend_to,
        'receiver': RECEIVER,
        'requester': requester,
        'maxPriceAccepted': max_price_accepted,
        'resourceType': RESOURCE_TYPE,
    }
    
    response = requests.post(url, json=body)
    return response.json()

def get_signed_tx(total_estimate_trx: int) -> Dict[str, Any]:
    """Get signed transaction"""
    # This is a placeholder. In real implementation, you would use TronWeb Python SDK
    return {
        'visible': False,
        'txID': '...',
        'raw_data': {
            'contract': [{
                'parameter': {
                    'value': {
                        'amount': total_estimate_trx,
                        'owner_address': REQUESTER_ADDRESS,
                        'to_address': TRONSAVE_RECEIVER_ADDRESS
                    },
                    'type_url': 'type.googleapis.com/protocol.TransferContract'
                },
                'type': 'TransferContract'
            }]
        }
    }

def send_extend_request(extend_to: int, max_price_accepted: int) -> Dict[str, Any]:
    """Send extend request"""
    url = f"{TRONSAVE_API_URL}/v2/extend-request"
    
    estimate_response = get_estimate_extend_data(REQUESTER_ADDRESS, extend_to, max_price_accepted)
    extend_data = estimate_response.get('data', {}).get('extendData', [])

    if extend_data:
        total_estimate_trx = estimate_response['data']['totalEstimateTrx']
        signed_tx = get_signed_tx(total_estimate_trx)

        body = {
            'extendData': extend_data,
            'receiver': RECEIVER,
            'signedTx': signed_tx
        }
        
        response = requests.post(url, json=body)
        return response.json()

    return {}

def main() -> None:
    """Main function"""
    try:
        extend_to = int(time.time()) + (3 * 86400)  # Extend to 3 next days
        max_price_accepted = 900
        response = send_extend_request(extend_to, max_price_accepted)
        print("Response:", response)
    except Exception as e:
        print(f"Error: {str(e)}")

if __name__ == "__main__":
    main() 
```

{% hint style="warning" %}
The Python `get_signed_tx()` function above is a placeholder. Use a TRON signing library such as `tronpy` to build and sign the TRX transfer before sending the extend request. Note that the example also references `time.time()` but the `import time` line is commented out — uncomment or add `import time` before running.
{% endhint %}
{% endtab %}
{% endtabs %}

## How it works

1. **Estimate.** `GetEstimateExtendData` calls `/v2/get-extendable-delegates` with `extendTo`, `receiver`, `requester`, an optional `maxPriceAccepted`, and `resourceType`. The response includes `extendData` (the delegates that can be extended) and `totalEstimateTrx` (the cost in SUN).
2. **Sign.** If `extendData` is non-empty, `GetSignedTx` builds a TRX transfer of `totalEstimateTrx` from `REQUESTER_ADDRESS` to `TRONSAVE_RECEIVER_ADDRESS` and signs it locally with `PRIVATE_KEY`.
3. **Submit.** `SendExtendRequest` posts `extendData`, `receiver`, and the `signedTx` to `/v2/extend-request`. On success the response contains an `orderId`.

{% hint style="info" %}
`maxPriceAccepted` is an upper bound on the price (in SUN) you are willing to pay to extend. If the order book price exceeds it, fewer delegates may be returned in `extendData`.
{% endhint %}

## Next steps

- [Extend Orders API reference](../../api-reference/extend-orders/README.md) — full request and response schemas for `get-extendable-delegates` and `extend-request`.
- [Authentication](../../authentication.md) — compare the private-key/signed-transaction flow with the API key flow.
- [Order types](../../../concepts/order-types.md) — understand how extend orders relate to other order types.
