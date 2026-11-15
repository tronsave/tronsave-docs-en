---
description: Legacy v0 flow for extending TronSave resource delegations with an API Key — check extendable delegates, then create extend requests.
---

# Extend with REST API (v0)

{% hint style="warning" %}
**Legacy API.** The v0 endpoints described here are kept for backwards compatibility. New integrations should use the current [API Reference](../api-reference/README.md) instead.
{% endhint %}

To use this feature you must have an API Key. See [Authentication](../authentication.md) for how to get one and how it is tied to your Internal Account.

The extend flow has two steps:

1. **Check extendable delegates** — estimate which delegations can be extended, the price, and the total TRX cost.
2. **Create the extend request** — submit the `extend_data` from step 1 to actually extend.

{% hint style="info" %}
A runnable Postman collection for this flow is available: [Extend order with API Key](https://www.postman.com/tronsave/tronsave/folder/z5xq9ux/extend-order-with-api-key).
{% endhint %}

## Step 1: Check all extendable delegates

<mark style="color:orange;">`POST`</mark> `https://api.tronsave.io/v0/get-extendable-delegates`

Check extendable delegates with your API Key.

Rate limit: **1** request per **1** second.

#### Headers

<table><thead><tr><th width="134">Name</th><th width="143">Type</th><th>Description</th></tr></thead><tbody><tr><td>apikey<mark style="color:red;">*</mark></td><td>String</td><td>TronSave API Key tied to your Internal Account</td></tr></tbody></table>

#### Request Body

<table><thead><tr><th width="166">Name</th><th width="167">Type</th><th>Description</th></tr></thead><tbody><tr><td>extend_to<mark style="color:red;">*</mark></td><td>String</td><td>Time in milliseconds you want to extend to</td></tr><tr><td>max_price</td><td>Number</td><td>Maximum price you want to pay to extend</td></tr><tr><td>receiver</td><td>String</td><td>The address that received the resource delegation</td></tr></tbody></table>

{% tabs %}
{% tab title="200: OK Success" %}
```javascript
  {
            "extend_order_book": [
                {
                    "price": 133,
                    "value": 64319
                },
                ...
            ], //Overview extend energy amount at every single price
            "total_delegate_amount": 64319, 
            //Total current delegate of receiver address in tronsave
            "total_available_extend_amount": 64319,
            //Total available delegate of receiver address in tronsave
            "total_estimate_trx": 8554427,
            //Estimate TRX payout if using extend_data below to create extend request
            "your_balance": 20000000,
            //api key's internal balance 
            "is_able_to_extend": true,
            //Compare balance and total_estimate_trx 
            "extend_data": [
                {
                    "delegator": "TMN2uTdy6rQYaTm4A5g732kHRf72tKsA4w",
                    "is_extend": true,
                    "extra_amount": 0,
                    "extend_to": 1728459019000
                }
            ]
            //Extend data that are used to create extend requests below
}
```
{% endtab %}

{% tab title="401: Unauthorized Invalid api key" %}
```
{
    "API_KEY_REQUIRED": "Missing api key in headers",
    "INVALID_API_KEY":"api key not correct"
}
```
{% endtab %}

{% tab title="429: Too Many Requests Rate limit reached" %}
```
{
    "RATE_LIMIT": "Rate limit reached",
}
```
{% endtab %}
{% endtabs %}

The `extend_data` array in the response is what you pass to Step 2 to create the actual extend requests.

### Example

{% tabs %}
{% tab title="Body" %}
```java
{
    "extend_to":1728704969000,
    "max_price":165,
    "receiver":"TFwUFWr3QV376677Z8VWXxGUAMF11111111"
}
```
{% endtab %}

{% tab title="Headers" %}
```java
{
  "apikey": <YOUR_API_KEY>
}
```
{% endtab %}

{% tab title="Success Response" %}
```java
{
    "extend_order_book": [
        {
            "price": 108,
            "value": 100000
        },
        {
            "price": 122,
            "value": 200000
        }
    ],
    "total_delegate_amount": 500000,
    "total_available_extend_amount": 300000,
    "total_estimate_trx": 24426224,
    "is_able_to_extend": true,
    "your_balance": 37780396,
    "extend_data": [
        {
            "delegator": "TQBV7xU489Rq8ZCsYi72zBhJM2222222",
            "is_extend": true,
            "extra_amount": 0,
            "extend_to": 1728704969000
        },
        {
            "delegator": "TMN2uTdy6rQYaTm4A5g732kHR333333333",
            "is_extend": true,
            "extra_amount": 0,
            "extend_to": 1728704969000
        }
    ]
}
```
{% endtab %}
{% endtabs %}

### Example code

{% tabs %}
{% tab title="Javascript" %}
<pre class="language-javascript"><code class="lang-javascript">const GetEstimateExtendData = async () => {
    const url = `https://api.tronsave.io/v0/get-extendable-delegates`
<strong>    const body = {
</strong>        "extend_to": extend_to, //time in milliseconds you want to extend to
        "receiver": RECEIVER,   //the address that receives resource delegate
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
         * Example response:
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
</code></pre>
{% endtab %}

{% tab title="cURL" %}
```powershell
curl --location 'https://api.tronsave.io/v0/get-extendable-delegates' \
--header 'apikey: {{apikey}}' \
--data '
{
 "extend_to": {{extend_timestamp_in_millisecs}}, 
 "receiver": {{receiver_address}},   
 "max_price": {{max_price_per_unit_want_to_pay}}, 
}
'
```
{% endtab %}
{% endtabs %}

## Step 2: Create an extend request by API Key

<mark style="color:orange;">`POST`</mark> `https://api.tronsave.io/v0/internal-extend-request`

Create a new extend request order with your API Key.

Rate limit: **15** requests per **1** second.

#### Headers

<table><thead><tr><th width="135">Name</th><th width="154">Type</th><th>Description</th></tr></thead><tbody><tr><td>apikey<mark style="color:red;">*</mark></td><td>String</td><td>TronSave API Key tied to your Internal Account</td></tr></tbody></table>

#### Request Body

<table><thead><tr><th width="170">Name</th><th width="152">Type</th><th>Description</th></tr></thead><tbody><tr><td>receiver<mark style="color:red;">*</mark></td><td>String</td><td>The address that received the resource</td></tr><tr><td>extend_data<mark style="color:red;">*</mark></td><td>Array</td><td>Array of extend data. Take it from the response of the estimate extendable delegates API (Step 1)</td></tr></tbody></table>

{% tabs %}
{% tab title="200: OK Success" %}
Returns an array of order IDs on success.

```json
[<order_id_1>,<order_id_2>,...]
```
{% endtab %}

{% tab title="401: Unauthorized Invalid api key" %}
```json
{
    "API_KEY_REQUIRED": "Missing api key in headers",
    "INVALID_API_KEY":"api key not correct"
}
```
{% endtab %}

{% tab title="429: Too Many Requests Rate limit reached" %}
```json
{
    "RATE_LIMIT": "Rate limit reached",
}
```
{% endtab %}
{% endtabs %}

### Example

{% tabs %}
{% tab title="Body" %}
```json
{
    "extend_data":[
        {
            "delegator": {{some_delegator_address}},
            "is_extend": true,
            "extra_amount": 0,
            "extend_to": {{extend_timestamp_in_millisecs}}
        },
        ...
    ],
    "receiver":{{receiver_address}}
}
```
{% endtab %}

{% tab title="Headers" %}
```json
{
  "apikey": <YOUR_API_KEY>
}
```
{% endtab %}

{% tab title="Success Response" %}
```json
["651d2306e55c073f6ca0992e","651d2306e55c073f6ca09923",...]
```
{% endtab %}
{% endtabs %}

### Example code

{% tabs %}
{% tab title="Javascript" %}
```javascript
const SendInternalExtendRequest = async () => {
    const url = `https://api.tronsave.io/v0/internal-extend-request`
    const body = {
            "extend_data": [    
                {
                    "delegator": {{some_delegator_address}},
                    "is_extend": true,
                    "extra_amount": 0,
                    "extend_to": {{extend_timestamp_in_millisecs}}
                }
            ],
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
```
{% endtab %}

{% tab title="cURL" %}
```powershell
curl --location 'https://api.tronsave.io/v0/internal-extend-request' \
--header 'apikey: {{apikey}}' \
--data '
{
    "extend_data":[
        {
            "delegator": {{some_delegator_address}},
            "is_extend": true,
            "extra_amount": 0,
            "extend_to": {{extend_timestamp_in_millisecs}}
        },
        ...
    ],
    "receiver":{{receiver_address}}
}
'
```
{% endtab %}
{% endtabs %}

## Next steps

* [Buy with REST API (v0)](buy/api-key.md) — the legacy buy flow.
* [API Reference](../api-reference/README.md) — the current, recommended API.
* [Authentication](../authentication.md) — get and manage your API Key.
