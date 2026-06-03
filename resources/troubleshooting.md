---
description: >-
  Fixes for the most common problems when buying Energy or integrating with
  TronSave — orders not filling, low balance, rate limits, ZapBuy delivery, and
  wrong network.
---

# Troubleshooting

This page collects the most common integration and buying problems, the exact error strings you'll see, and how to resolve them. For the full machine-readable list of API error codes, see [Errors & Rate Limits](../developers/errors-and-rate-limits.md).

## Order isn't filling

When you create an order that requires an immediate full match but the market can't supply 100% of it, the API returns **HTTP 400** with:

```json
{
    "CANNOT_FULFILLED": "The order requires an immediate full match, but the system cannot fulfill 100% of it."
}
```

This usually means one of the following:

* **Not enough supply at your price.** The market doesn't have enough Energy available at or below your `unitPrice`.
* **Your price ceiling is too low.** If the estimated price exceeds `options.maxPriceAccepted`, you'll instead get `PRICE_EXCEED_MAX_PRICE_REQUIRED`.

**How to fix:**

* Allow a partial fill (`allowPartialFill`) so the order matches what's available now instead of requiring 100% immediately.
* Re-check the price with [Estimate TRX](../developers/api-reference/buy-resources/api-key/estimate-trx.md) before submitting, then set `unitPrice` to the returned value.
* For large rentals the market can't match at once, use a [Smart Order](../concepts/order-types.md) (minimum 10,000,000 Energy, minimum 3-day duration), which keeps matching more over time.
* If you don't need immediate delivery, use a [Pending Order](../guides/buy/on-the-website/pending-order.md), which waits in the order book until the market can match your price.

{% hint style="info" %}
On the bot/Telegram, orders are always submitted at the **MEDIUM** price to guarantee a 100% match, so a bot purchase can cost more than a website order where you set the lowest possible price. The two prices only diverge when the system is short on resources.
{% endhint %}

### "A pending order already exists"

```json
{
    "MUST_BE_WAIT_PREVIOUS_ORDER_FILLED": "A pending order with the same parameters already exists in the system."
}
```

You already have a pending order with identical parameters. Wait for it to fill (or cancel it) before submitting the same order again, or change a parameter (for example, the `unitPrice`).

## Balance too low

Orders are paid from your [Internal Account](../concepts/glossary.md) balance. If it's insufficient you'll see **HTTP 400**:

```json
{
    "INTERNAL_BALANCE_ACCOUNT_TOO_LOW": "Balance is not enough"
}
```

**How to fix:** top up your Internal Account before creating the order. If the account itself doesn't exist yet, you'll instead see:

```json
{
    "INTERNAL_ACCOUNT_NOT_FOUND": "internal account does not exist"
}
```

Create/fund the account first, then retry.

## Rate limited (HTTP 429)

Most endpoints allow **15 requests per second**; a few are lower. Exceeding the limit returns **HTTP 429**:

```json
{
    "RATE_LIMIT": "Rate limit reached"
}
```

**How to fix:** back off and retry within your per-second budget. A simple exponential backoff (wait 1s, then 2s, then 4s) is usually enough. See [Errors & Rate Limits](../developers/errors-and-rate-limits.md) for the per-endpoint limits.

## Authentication failures (HTTP 401)

```json
{
    "API_KEY_REQUIRED": "Missing api key in headers",
    "INVALID_API_KEY": "api key not correct"
}
```

* `API_KEY_REQUIRED` — the `apikey` header is missing from the request.
* `INVALID_API_KEY` — the key is present but wrong (typo, revoked, or from a different environment).

**How to fix:** confirm the `apikey` header is set and matches a valid key. See [Authentication](../developers/authentication.md).

{% hint style="warning" %}
A key issued on Testnet will not work against Mainnet and vice versa. Make sure the key matches the [environment](../developers/environments.md) you're calling.
{% endhint %}

## Energy not received via ZapBuy

With [ZapBuy](../guides/buy/zapbuy.md), you send TRX to the TronSave bot address and a 1-hour Energy rental is created automatically for the sending wallet. If no Energy arrives, check each of the following:

* **You sent at least the minimum.** ZapBuy requires renting **at least 65,000 Energy**. Requests below that are **ignored,** and no order is created.
* **You sent from a regular wallet.** Energy is delegated back to the sending address, which must be a normal wallet — **not a smart contract or exchange wallet**.
* **The transaction confirmed.** It must be successfully confirmed on the TRON blockchain.
* **The order could be matched immediately.** ZapBuy only fills when Energy is available right now. If no match is found, you won't receive Energy.
*   **You sent the correct token to the correct address.** ZapBuy accepts **TRX only**, sent to the TronSave bot address:

    ```ini
    TLx8h8fjv5pyuxCu292ZgjbU14XZSiLGg4
    ```

If you've met all of the above and still didn't receive Energy, contact TronSave **immediately** with your transaction details to request help or a refund: [https://t.me/wantingtrx](https://t.me/wantingtrx).

## Wrong network

Sending to the wrong network is the most common source of "lost" funds and undelivered Energy.

* **TRX sent on a non-TRON network.** ZapBuy and Internal Account deposits only accept native **TRX on the TRON network**. Funds sent via another chain (or a wrapped/bridged token) will not be credited.
* **API calls hitting the wrong environment.** Testnet and Mainnet have separate base URLs and separate API keys. Calling Mainnet with a Testnet key (or URL) produces `INVALID_API_KEY` or unexpected empty results. Verify your base URL and key against [Environments](../developers/environments.md).

{% hint style="warning" %}
Always double-check the **target address** you're delegating Energy to. A discrepancy in the target address can also explain a price difference between a bot purchase and the website.
{% endhint %}

## Quick reference

<table><thead><tr><th width="120">HTTP</th><th width="320">Code</th><th>Fix</th></tr></thead><tbody><tr><td>400</td><td><code>CANNOT_FULFILLED</code></td><td>Allow partial fill, adjust price, or use a Smart/Pending order.</td></tr><tr><td>400</td><td><code>PRICE_EXCEED_MAX_PRICE_REQUIRED</code></td><td>Re-estimate and raise <code>options.maxPriceAccepted</code>.</td></tr><tr><td>400</td><td><code>MUST_BE_WAIT_PREVIOUS_ORDER_FILLED</code></td><td>Wait for the existing order, or change a parameter.</td></tr><tr><td>400</td><td><code>INTERNAL_BALANCE_ACCOUNT_TOO_LOW</code></td><td>Top up your Internal Account.</td></tr><tr><td>400</td><td><code>INTERNAL_ACCOUNT_NOT_FOUND</code></td><td>Create/fund the Internal Account first.</td></tr><tr><td>401</td><td><code>API_KEY_REQUIRED</code> / <code>INVALID_API_KEY</code></td><td>Set a valid <code>apikey</code> header for the right environment.</td></tr><tr><td>429</td><td><code>RATE_LIMIT</code></td><td>Back off and retry with exponential backoff.</td></tr></tbody></table>

## Next steps

* [Errors & Rate Limits](../developers/errors-and-rate-limits.md) — full error-code reference.
* [Order Types](../concepts/order-types.md) — pick the right order to avoid fill failures.
* [ZapBuy](../guides/buy/zapbuy.md) · [Environments](../developers/environments.md) · [Authentication](../developers/authentication.md)
