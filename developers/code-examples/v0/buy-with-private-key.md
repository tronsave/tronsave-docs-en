---
description: Full working example for buying Energy via the TronSave v0 API by signing a TRX transaction with your own private key.
---

# Buy Energy with a Private Key (v0)

{% hint style="warning" %}
**Legacy example.** This page uses the **v0** TronSave API and **TronWeb 5.3.2**. It is kept for reference and backwards compatibility. For new integrations, use the current Signed Transaction flow in [Buy Resources](../../api-reference/buy-resources/README.md) and the [Developer Quickstart](../../quickstart.md).
{% endhint %}

This example shows the complete Signed Transaction flow: estimate the cost, sign a TRX transfer to the TronSave receiver address with your private key, and create the buy order — all without depositing TRX into an internal account.

## Requirement

**TronWeb version 5.3.2**

```bash
npm i tronweb@5.3.2 @noble/secp256k1@1.7.1
```

Read more: [TronWeb 5.3.2 release notes](https://tronweb.network/docu/docs/5.3.2/Release%20Note/)

## Configuration

The example reads four endpoint/address constants. The values differ between mainnet and the Nile testnet:

<table>
<thead>
<tr><th>Constant</th><th>Mainnet</th><th>Testnet (Nile)</th></tr>
</thead>
<tbody>
<tr><td><code>TRONSAVE_RECEIVER_ADDRESS</code></td><td><code>TWZEhq5JuUVvGtutNgnRBATbF8BnHGyn4S</code></td><td><code>TATT1UzHRikft98bRFqApFTsaSw73ycfoS</code></td></tr>
<tr><td><code>TRON_FULLNODE</code></td><td><code>https://api.trongrid.io</code></td><td><code>https://api.nileex.io</code></td></tr>
<tr><td><code>TRONSAVE_API_URL</code></td><td><code>https://api.tronsave.io</code></td><td><code>https://api-dev.tronsave.io</code></td></tr>
</tbody>
</table>

Set `PRIVATE_KEY`, `REQUEST_ADDRESS`, and `TARGET_ADDRESS` before running. `BUY_AMOUNT` and `DURATION` can be changed to fit your order.

## Full example

{% tabs %}
{% tab title="Javascript" %}
```javascript
const TronWeb = require('tronweb')

const TRONSAVE_RECEIVER_ADDRESS = "TWZEhq5JuUVvGtutNgnRBATbF8BnHGyn4S" //in testnet mode change it to "TATT1UzHRikft98bRFqApFTsaSw73ycfoS"
const PRIVATE_KEY = "your_private_key" //Change it
const TRON_FULLNODE = "https://api.trongrid.io"  //in testnet mode change it to "https://api.nileex.io"
const TRONSAVE_API_URL = "https://api.tronsave.io" //in testnet mode change it to "https://api-dev.tronsave.io"

const REQUEST_ADDRESS = "your_request_address" //Change it
const TARGET_ADDRESS = "your_target_address" //Change it
const BUY_AMOUNT = 100000 //Chanageable
const DURATION = 3 * 86400 * 1000 //3 days.Changeable

const tronWeb = new TronWeb({
    fullNode: TRON_FULLNODE,
    solidityNode: TRON_FULLNODE,
    eventServer: TRON_FULLNODE,
    privateKey: PRIVATE_KEY,
})

const GetEstimate = async (request_address, target_address, amount, duration) => {
    const url = TRONSAVE_API_URL + "/v0/estimate-trx"
    //see more at https://docs.tronsave.io/buy-energy-on-telegram/using-api-key-to/buy-energy
    const body = {
        "amount": amount,
        "buy_energy_type": "MEDIUM",
        "duration_millisec": duration,
        "request_address": request_address,
        "target_address": target_address || request_address,
        "is_partial": true
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
     * @link  https://docs.tronsave.io/market/how-to-buy-energy/buy-on-rest-api
   {
        "unit_price": 45,
        "duration_millisec": 259200000,
        "available_energy": 4298470,
        "estimate_trx": 13500000,
    }
     */
    return response
}

const GetSignedTransaction = async (estimate_trx, request_address) => {
    const dataSendTrx = await tronWeb.transactionBuilder.sendTrx(TRONSAVE_RECEIVER_ADDRESS, estimate_trx, request_address)
    const signed_tx = await tronWeb.trx.sign(dataSendTrx, PRIVATE_KEY);
    return signed_tx
}

const CreateOrder = async (signed_tx, target_address, unit_price, duration) => {
    const url = TRONSAVE_API_URL + "/v0/buy-energy"
    //see more at https://docs.tronsave.io/buy-energy-on-telegram/using-api-key-to/buy-energy
    const body = {
        "resource_type": "ENERGY",
        "unit_price": unit_price,
        "allow_partial_fill": true,
        "target_address": target_address,
        "duration_millisec": duration,
        "tx_id": signed_tx.txID,
        "signed_tx": signed_tx
    }
    const data = await fetch(url, {
        method: "POST",
        headers: {
            "content-type": "application/json",
        },
        body: JSON.stringify(body)
    })
    const response = await data.text()
    //Example response 
    // @link  https://docs.tronsave.io/market/how-to-buy-energy/buy-on-rest-api
    // "651d2306e55c073f6ca0992e" //order id in tronsave
    return response
}

const BuyEnergyUsingPrivateKey = async () => {
    //Step 1: Estimate the cost of buy order
    const estimate_data = await GetEstimate(REQUEST_ADDRESS, TARGET_ADDRESS, BUY_AMOUNT, DURATION)
    const { unit_price, estimate_trx, available_energy } = estimate_data
    console.log(estimate_data)
    /*
    {
        "unit_price": 45,
        "duration_millisec": 259200000,
        "available_energy": 4298470,
        "estimate_trx": 13500000,
    }
     */
    const is_ready_fulfilled = available_energy >= BUY_AMOUNT //if available_energy equal buy_amount it means that your order can be fulfilled 100%
    if (is_ready_fulfilled) {
        //Step 2: Build signed transaction by using private key
        const signed_tx = await GetSignedTransaction(estimate_trx, REQUEST_ADDRESS)
        console.log(signed_tx);
        /*
        {
            "resource_type": "ENERGY",
            "unit_price": 45,
            "allow_partial_fill": true,
            "target_address": "TM6ZeEgpefyGWeMLuzSbfqTGkPv8Z6Jm4X",
            "duration_millisec": 259200000,
            "tx_id": "446eed36e31249b98b201db2e81a3825b185f1a3d8b2fea348b24fc021e58e0d",
            "signed_tx": {
            "visible": false,
            "txID": "446eed36e31249b98b201db2e81a3825b185f1a3d8b2fea348b24fc021e58e0d",
            "raw_data": {
                "contract": [
                {
                    "parameter": {
                    "value": {
                        "amount": 13500000,
                        "owner_address": "417a0d868d1418c9038584af1252f85d486502eec0",
                        "to_address": "41055756f33f419278d9ea059bd2b21120e6add748"
                    },
                    "type_url": "type.googleapis.com/protocol.TransferContract"
                    },
                    "type": "TransferContract"
                }
                ],
                "ref_block_bytes": "0713",
                "ref_block_hash": "6c5f7686f4176139",
                "expiration": 1691465106000,
                "timestamp": 1691465046758
            },
            "raw_data_hex": "0a02071322086c5f7686f417613940d084b5999d315a68080112640a2d747970652e676f6f676c65617069732e636f6d2f70726f746f636f6c2e5472616e73666572436f6e747261637412330a15417a0d868d1418c9038584af1252f85d486502eec0121541055756f33f419278d9ea059bd2b21120e6add74818e0fcb70670e6b5b1999d31",
            "signature": ["xxxxxxxxx"]
            }
        }
         */
        //Step 3: Create order
        const dataCreateOrder = await CreateOrder(signed_tx, TARGET_ADDRESS, unit_price, DURATION)
        console.log(dataCreateOrder);
    }
}

BuyEnergyUsingPrivateKey()
```
{% endtab %}
{% endtabs %}

## How it works

1. **Estimate** — `POST /v0/estimate-trx` returns the `unit_price`, `available_energy`, and `estimate_trx` (the TRX cost, in SUN) for your requested `BUY_AMOUNT` and `DURATION`.
2. **Sign** — Build a TRX transfer of `estimate_trx` to `TRONSAVE_RECEIVER_ADDRESS` and sign it locally with your private key. No TRX is moved until the order is created.
3. **Create order** — `POST /v0/buy-energy` submits the signed transaction. On success the response is the order ID, e.g. `"651d2306e55c073f6ca0992e"`.

The example only proceeds to sign and create the order when `available_energy >= BUY_AMOUNT`, ensuring the order can be filled 100%.

{% hint style="warning" %}
Never commit your `PRIVATE_KEY` to source control or share it. Load it from a secure environment variable or secret manager in production.
{% endhint %}

## Next steps

- Migrate to the current API: [Buy Resources](../../api-reference/buy-resources/README.md)
- Compare auth methods: [Authentication](../../authentication.md)
- Learn the rental model: [Concepts → Rental Model](../../../concepts/rental-model.md)
