---
description: >-
  Retrieve detailed information about a specific delegation using its Delegate
  ID.
---

# Get Delegate Detail

{% hint style="warning" %}
**Whitelist required.** The Sell API is restricted to whitelisted wallets. Log in to TronSave and send your wallet address to support to add it to the whitelist. Once whitelisted, use the API Key generated from that wallet address to call the Sell endpoints.

Contact support on Telegram: [**@wantingtrx**](https://t.me/wantingtrx)
{% endhint %}

## Endpoint

Returns detailed information about a specific delegation.

<mark style="color:blue;">**`GET`**</mark> **`https://api.tronsave.io/v2/delegate/:delegateId`**

The `:delegateId` returned when selling the resource.

{% hint style="info" %}
**Rate limit:** 5 requests per 1 second.
{% endhint %}

### Headers

<table><thead><tr><th width="160">Name</th><th width="120">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>apikey</code> <mark style="color:red;">*</mark></td><td>String</td><td>TronSave API Key (must belong to a whitelisted address). See <a href="../../authentication.md">Authentication</a>.</td></tr></tbody></table>

## Path parameters

<table><thead><tr><th width="124">Name</th><th width="109">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>delegateId</code></td><td>string</td><td>The unique ID of the delegation to retrieve. (<a href="sell-resources-1.md">Sell the resources</a>)</td></tr></tbody></table>

### Response

```javascript
{
  "error": false,
  "message": "Success",
  "data": {
    "id": "string",                 // Delegate ID
    "delegator": {
      "address": "string"           // TRON address of the delegator
    },
    "receiver": {
      "address": "string"           // TRON address of the receiver
    },
    "order": {
      "id": "string"                // ID of the order
    },
    "resourceType": "string",       // Resource type: "ENERGY" or "BANDWIDTH"
    "resourceAmount": "number",     // Amount of delegated resource
    "trxAmount": "number",          // TRX amount to delegate(SUN)
    "durationSec": "number",        // Delegation duration in seconds
    "unitPrice": "number",          // Unit price (SUN)
    "typeDelegate": "string",       // Delegation type, e.g. "AUTO", "MANUAL" or "EXTEND"
    "status": "string",             // Delegation status, e.g. "SUCCESS"
    "paidAmount": "string",         // Amount paid (SUN)
    "paidFee": "string",            // Payment fee (SUN)
    "paidAt": "number",             // Payment timestamp (Unix timestamp)
    "paymentAddress": "string",     // TRON address used for payment
    "paidTxid": {
      "txid": "string"              // Payment transaction ID
    },
    "delegateAt": "number",        // Delegation timestamp (Unix timestamp)
    "delegateTxid": {
      "txid": "string"              // Delegation transaction ID
    },
    "smartMatchingInfo": "object | null", // Smart Matching details, if applicable
    "extendInfo": "object | null",         // Extension details, if applicable
    "fastInfo": "object | null"            // Fast delegation details, if applicable
  }
}
```

**Example response**

```javascript
{
    "error": false,
    "message": "Success",
    "data": {
        "id": "6ae083908a976b17d6714777",
        "delegator": {
            "address": "TURL8VVSs9Z8ryd2r8vLZsqPtzDD383838"
        },
        "receiver": {
            "address": "TWrUyds2aPLhRgwwj3dzaEL8Z7udcbcbcb"
        },
        "order": {
            "id": "6ae0839036d832402c293add"
        },
        "resourceType": "BANDWIDTH",
        "resourceAmount": 1001,
        "trxAmount": 1585700000,
        "durationSec": 300,
        "unitPrice": 600,
        "typeDelegate": "AUTO",
        "status": "SUCCESS",
        "paidAmount": "450450",
        "paidFee": "150150",
        "paidAt": 1793145599,
        "paymentAddress": "TURL8VVSs9Z8ryd2r8vLZsqPtzDD383838",
        "paidTxid": {
            "txid": "ac9299d9def7b3f2be93ceae9e78cecd4c957311bc2e1f4e65b4dfbacb5ee522"
        },
        "delegateAt": 1793098640,
        "delegateTxid": {
            "txid": "12b9d9bcbf01fd29555276aef58dfba0c457e68dbf4698d6ca093b54c37a8599"
        },
        "smartMatchingInfo": null,
        "extendInfo": null,
        "fastInfo": null
    }
}
```



## Next steps

* New to the API? Start with the [Quickstart](../../quickstart.md) and [Authentication](../../authentication.md).
* Learn the difference between [Energy and Bandwidth](../../../concepts/energy-and-bandwidth.md) and review [Order Types](../../../concepts/order-types.md).
* Looking to buy instead of sell? See [Buy Resources](../buy-resources/).
