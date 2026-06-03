---
description: Rate limits, HTTP status codes, and error payloads returned by the TronSave API.
---

# Errors & Rate Limits

Every TronSave API endpoint shares the same rate-limiting policy and error-response shape. This page documents both so you can build robust retry and error-handling logic once and reuse it across all calls.

## Rate limits

Most endpoints allow a default of **15 requests per second**.

{% hint style="warning" %}
The [Sell Resources](api-reference/sell-resources.md) endpoints are limited to **2–3 requests per second**. All other endpoints use the default of 15 requests per second.
{% endhint %}

The current limit for each endpoint is documented in its API reference page (for example, the [Create Order](api-reference/buy-resources/api-key/create-order.md) and [Estimate TRX](api-reference/buy-resources/api-key/estimate-trx.md) endpoints state `Rate limit: 15 requests per 1 second`).

When you exceed the limit, the API returns **HTTP 429**:

```json
{
    "error": true,
    "message": "Rate limit reached"
}
```

{% hint style="info" %}
Handle `429` by backing off and retrying. A simple exponential backoff (e.g. wait 1s, then 2s, then 4s) is usually sufficient to stay within the per-second budget.
{% endhint %}

## Response shape

Successful responses use a consistent envelope: `error` is `false`, `message` is `"Success"`, and the payload is under `data`.

```json
{
    "error": false,
    "message": "Success",
    "data": {
        "orderId": "6818426a65fa8ea36d119d2c"
    }
}
```

Error responses return a non-2xx HTTP status code. Most application errors use the same envelope with `error: true`, a machine-readable code embedded in `message` (e.g. `TSAS:106 API_KEY_REQUIRED`), and `data: null`:

```json
{
    "error": true,
    "message": "TSAS:106 API_KEY_REQUIRED",
    "data": null
}
```

Schema-validation failures are produced by the HTTP layer and use a different shape, naming the offending field in `message`:

```json
{
    "statusCode": 400,
    "code": "FST_ERR_VALIDATION",
    "error": "Bad Request",
    "message": "body must have required property 'receiver'"
}
```

## Error codes

The table below lists the error codes observed across the API, grouped by HTTP status. Use the **code** (the JSON key) for programmatic handling — the **message** text may change.

<table>
  <thead>
    <tr><th width="90">HTTP</th><th width="320">Code</th><th>Message</th></tr>
  </thead>
  <tbody>
    <tr><td>400</td><td><code>MISSING_PARAMS</code></td><td>Missing some params in body</td></tr>
    <tr><td>400</td><td><code>INVALID_PARAMS</code></td><td>Some params are invalid</td></tr>
    <tr><td>400</td><td><code>MIN_PRICE_INVALID</code></td><td>minPrice is less than the system's minimum price or not in the correct format.</td></tr>
    <tr><td>400</td><td><code>INTERNAL_ACCOUNT_NOT_FOUND</code></td><td>internal account does not exist</td></tr>
    <tr><td>400</td><td><code>INTERNAL_BALANCE_ACCOUNT_TOO_LOW</code></td><td>Balance is not enough</td></tr>
    <tr><td>400</td><td><code>CANNOT_FULFILLED</code></td><td>The order requires an immediate full match, but the system cannot fulfill 100% of it.</td></tr>
    <tr><td>400</td><td><code>MUST_BE_WAIT_PREVIOUS_ORDER_FILLED</code></td><td>A pending order with the same parameters already exists in the system.</td></tr>
    <tr><td>400</td><td><code>PRICE_EXCEED_MAX_PRICE_REQUIRED</code></td><td>The price of the order exceeds the maximum price accepted.</td></tr>
    <tr><td>401</td><td><code>API_KEY_REQUIRED</code></td><td>Missing api key in headers</td></tr>
    <tr><td>401</td><td><code>INVALID_API_KEY</code></td><td>api key not correct</td></tr>
    <tr><td>429</td><td><code>RATE_LIMIT</code></td><td>Rate limit reached</td></tr>
  </tbody>
</table>

{% hint style="info" %}
Not every code applies to every endpoint. For example, `CANNOT_FULFILLED` and `PRICE_EXCEED_MAX_PRICE_REQUIRED` are only returned when creating an order. Individual API reference pages list the codes specific to each endpoint.
{% endhint %}

## Example error payloads

{% tabs %}
{% tab title="400 Bad Request" %}
Schema validation (a required field is missing or invalid — the message names the field):

```json
{
    "statusCode": 400,
    "code": "FST_ERR_VALIDATION",
    "error": "Bad Request",
    "message": "body must have required property 'receiver'"
}
```

Business-logic validation (returned by order-related endpoints) uses the standard envelope:

```json
{
    "error": true,
    "message": "TSAS:xxx INTERNAL_BALANCE_ACCOUNT_TOO_LOW",
    "data": null
}
```
{% endtab %}

{% tab title="401 Unauthorized" %}
Missing `apikey` header:

```json
{
    "error": true,
    "message": "TSAS:106 API_KEY_REQUIRED",
    "data": null
}
```

Invalid `apikey`:

```json
{
    "error": true,
    "message": "TSAS:107 INVALID_API_KEY",
    "data": null
}
```

{% hint style="info" %}
The legacy v0 API omits the `data` field: `{"error": true, "message": "TSAS:107 INVALID_API_KEY"}`.
{% endhint %}
{% endtab %}

{% tab title="404 Not Found" %}
Wrong route or path:

```json
{
    "message": "Route POST:/v2/... not found",
    "error": "Not Found",
    "statusCode": 404
}
```
{% endtab %}

{% tab title="429 Too Many Requests" %}
```json
{
    "error": true,
    "message": "Rate limit reached"
}
```
{% endtab %}
{% endtabs %}

## Handling common cases

- **`401` (`API_KEY_REQUIRED` / `INVALID_API_KEY`)** — Check that the `apikey` header is present and correct. See [Authentication](authentication.md).
- **`INTERNAL_BALANCE_ACCOUNT_TOO_LOW`** — Top up your [Internal Account](../concepts/glossary.md) before creating an order.
- **`CANNOT_FULFILLED`** — The market cannot fill the order in full at the requested price. Lower expectations (e.g. enable `allowPartialFill`) or adjust `unitPrice`.
- **`PRICE_EXCEED_MAX_PRICE_REQUIRED`** — The estimated price is above your `options.maxPriceAccepted`. Re-check the price with [Estimate TRX](api-reference/buy-resources/api-key/estimate-trx.md) before retrying.
- **`429` (`RATE_LIMIT`)** — Back off and retry within your per-second budget.

## Next steps

- [Authentication](authentication.md) — how to obtain and send your API Key.
- [API Reference](api-reference/README.md) — per-endpoint parameters and responses.
