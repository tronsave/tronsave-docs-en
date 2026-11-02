---
description: Check up to 100 TRON addresses at once — activated, not activated, contract, or invalid. No API key needed.
---

# Check Addresses

Check whether TRON addresses are activated before sending to them, delegating resources to them, or paying to activate them.

## Endpoint

<mark style="color:green;">**`POST`**</mark> **`https://api.tronsave.io/v0/helper/is-active-address-check`**

{% hint style="info" %}
**No API key required.** **Rate limit:** 5 requests per second. **Maximum:** 100 addresses per request.
{% endhint %}

## Request body

<table><thead><tr><th width="160">Name</th><th width="140">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>addresses</code><mark style="color:red;">*</mark></td><td>String[]</td><td>Up to 100 TRON addresses (base58, starting with <code>T</code>).</td></tr></tbody></table>

### Request body example

```json
{
    "addresses": [
        "TXYZ...address1",
        "TABC...address2"
    ]
}
```

## Responses

### 200 OK — Success

An array of status codes, **in the same order as the addresses you sent**:

| Code | Meaning |
| --- | --- |
| `0` | Not activated |
| `1` | Contract address |
| `2` | Activated |
| `3` | Not a valid TRON address |
| `4` | Could not be checked (retry) |

```json
[2, 0]
```

### 400 Bad Request

Returned when `addresses` is missing, is not an array, or has more than 100 entries.

```json
{
    "error": true,
    "message": "..."
}
```
