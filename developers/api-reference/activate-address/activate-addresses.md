---
description: Activate up to 100 TRON addresses in one API call. 1.5 TRX per inactive address, paid from your TronSave internal account; active addresses are free.
---

# Activate Addresses

Activate up to 100 TRON addresses in one request. TronSave checks each address, charges **1.5 TRX for every address that is not yet activated**, and activates them for you. Addresses that are already active are skipped and not charged. Duplicates in the request are removed.

To call this endpoint you need a TronSave API key tied to a prefunded internal account. See [Authentication](../../authentication.md).

## Endpoint

<mark style="color:orange;">**`POST`**</mark> **`https://api.tronsave.io/v0/helper/multi-active-address`**

{% hint style="info" %}
**Rate limit:** 5 requests per second. **Maximum:** 100 addresses per request.
{% endhint %}

## Headers

<table><thead><tr><th width="131">Name</th><th width="135">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>apikey</code><mark style="color:red;">*</mark></td><td>String</td><td>TronSave API key associated with the internal account that pays the activation fee.</td></tr></tbody></table>

## Request body

<table><thead><tr><th width="160">Name</th><th width="140">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>addresses</code><mark style="color:red;">*</mark></td><td>String[]</td><td>Up to 100 TRON addresses to activate.</td></tr></tbody></table>

### Request body example

```json
{
    "addresses": [
        "TXYZ...address1",
        "TABC...address2",
        "TDEF...address3"
    ]
}
```

## Responses

### 200 OK — Request accepted

The fee for the inactive addresses has been deducted and their activation is queued:

```json
{
    "message": "Success create 2 active address request(s)"
}
```

The number is how many addresses needed activating (and were charged). Activation completes asynchronously — use [Check Addresses](check-addresses.md) to confirm each address returns `2` (activated).

### Error responses

```json
{
    "error": true,
    "message": "..."
}
```

| Cause | What to do |
| --- | --- |
| Missing `apikey` header, or invalid key | Send a valid key — see [Authentication](../../authentication.md) |
| `addresses` missing, not an array, or more than 100 entries | Send 1–100 addresses per request |
| Internal account balance too low | Deposit TRX: you need 1.5 TRX × the number of inactive addresses |
| Too many requests | Stay under 5 requests per second |

## Example

```bash
curl -X POST https://api.tronsave.io/v0/helper/multi-active-address \
  -H "apikey: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"addresses": ["TXYZ...address1", "TABC...address2"]}'
```
