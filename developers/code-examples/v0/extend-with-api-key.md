---
description: Full v0 example showing how to extend a resource order using an API Key.
---

# Extend an Order with an API Key

This is a complete v0 example that extends an active resource delegation using a TronSave **API Key**. It estimates the cost via `get-extendable-delegates`, then submits the extension through `internal-extend-request`.

{% hint style="warning" %}
This is a legacy v0 example, kept for reference. For new integrations, use the current API. See [Extend Orders](../../api-reference/extend-orders/README.md) and the [Quickstart](../../quickstart.md).
{% endhint %}

## Prerequisites

You need an API Key. There are two ways to get one:

* [Get an API Key on the website](../../authentication.md)
* [Get an API Key on Telegram](../../authentication.md)

**Requirement: TronWeb version 5.3.2**

```bash
npm i tronweb@5.3.2 @noble/secp256k1@1.7.1
```

(Read more: [TronWeb 5.3.2 release notes](https://tronweb.network/docu/docs/5.3.2/Release%20Note/))

## Code

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const API_KEY = `your_api_key`;
const TRONSAVE_API_URL = "https://api.tronsave.io/v0"
const RECEIVER = "the_address_that_receive_resource"

/**
 * @param {*} extend_to time in milliseconds you want to extend to
 * @param {*} max_price number maximum price you want to pay to extend
 * @returns 
 */
const GetEstimateExtendData = async (extend_to, max_price) => {
    const url = TRONSAVE_API_URL + `/get-extendable-delegates`
    const body = {
        "extend_to": extend_to, //time in milliseconds you want to extend to
        "receiver": RECEIVER,   //the address that receives the resource delegate
        "max_price": max_price, //Optional. Number maximum price you want to pay to extend
    }
    const data = await fetch(url, {
        method: "POST",
        headers: {
            'apikey': API_KEY,
            "content-type": "application/json",
        },
        body: JSON.stringify(body)
    })
    const response = await data.json()
    /**
     * Example response 
     * @link  //TODO
       {
            "extend_order_book": [
                {
                    "price": 133,
                    "value": 64319
                }
            ],
            "total_delegate_amount": 64319,
            "total_available_extend_amount": 64319,
            "total_estimate_trx": 8554427,
            "is_able_to_extend": true,
            "your_balance": 20000000,
            "extend_data": [
                {
                    "delegator": "TMN2uTdy6rQYaTm4A5g732kHRf72tKsA4w",
                    "is_extend": true,
                    "extra_amount": 0,
                    "extend_to": 1728459019000
                }
            ]
        }
     */
    return response
}

/**
 * @param {*} extend_to time in milliseconds you want to extend to
 * @param {*} max_price number maximum price you want to pay to extend
 * @returns 
 */
const SendInternalExtendRequest = async (extend_to, max_price) => {
    const url = TRONSAVE_API_URL + `/internal-extend-request`
    const estimate_response = await GetEstimateExtendData(extend_to, max_price)
    if (estimate_response.extend_data && estimate_response.extend_data.length) { 
        const body = {
            "extend_data": estimate_response.extend_data,
            "receiver": RECEIVER,
        }
        const data = await fetch(url, {
            method: "POST",
            headers: {
                'apikey': API_KEY,
                "content-type": "application/json",
            },
            body: JSON.stringify(body)
        })
        const response = await data.json()
        /**
         * Example response 
         * @link  //TODO
           [<order_id>]
         */
        return response
    }
    return []
}

//Example run code
const ClientCode = () => { 
    const extend_to = +new Date() + 86400 * 1000 //Extend to 1 next day
    const max_price = 200

    SendInternalExtendRequest(extend_to, max_price).then(console.log)
}


ClientCode()
```
{% endtab %}
{% endtabs %}

## Next steps

* [Extend Orders API reference](../../api-reference/extend-orders/README.md)
* [Authentication](../../authentication.md)
