---
description: Where to check TronSave service status, how to report incidents, and what to know about uptime expectations.
---

# Status & SLA

This page tells you where to check whether TronSave is up, how to report a problem, and what to expect during incidents. It covers both the **website** and the **API** (Mainnet and the Nile testnet).

## Service status

{% hint style="info" %}
TronSave does not publish a dedicated status/uptime page. To check whether the service is up, test the relevant environment directly (see below) or ask in the community channel ([t.me/tronsave](https://t.me/tronsave)).
{% endhint %}

Without a status page, you can confirm whether a specific environment is reachable with a lightweight request against a read endpoint:

{% tabs %}
{% tab title="cURL" %}
```bash
# Mainnet
curl -s -o /dev/null -w "%{http_code}\n" https://api.tronsave.io/v2/user-info -H "apikey: YOUR_API_KEY"

# Nile testnet
curl -s -o /dev/null -w "%{http_code}\n" https://api-dev.tronsave.io/v2/user-info -H "apikey: YOUR_API_KEY"
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const TRONSAVE_API = "https://api.tronsave.io"; // or https://api-dev.tronsave.io

const res = await fetch(`${TRONSAVE_API}/v2/user-info`, {
  headers: { apikey: "YOUR_API_KEY" },
});

console.log(res.status); // 200 means the API is reachable and your key is valid
```
{% endtab %}
{% endtabs %}

A `200` confirms the API is reachable and your key is valid. A `5xx` or a connection timeout points to a service-side problem rather than a problem with your request. For the meaning of individual status codes, see [Errors & Rate Limits](../developers/errors-and-rate-limits.md).

## What "status" covers

TronSave has several independently operated surfaces. An issue on one does not necessarily mean the others are affected:

<table><thead><tr><th>Surface</th><th>Endpoint / URL</th><th>What it serves</th></tr></thead><tbody><tr><td><strong>Website (Mainnet)</strong></td><td><code>https://tronsave.io</code></td><td>Buy/sell UI, account dashboard</td></tr><tr><td><strong>Website (Nile)</strong></td><td><code>https://testnet.tronsave.io</code></td><td>Testnet UI</td></tr><tr><td><strong>API (Mainnet)</strong></td><td><code>https://api.tronsave.io</code></td><td>Production API</td></tr><tr><td><strong>API (Nile)</strong></td><td><code>https://api-dev.tronsave.io</code></td><td>Testnet API</td></tr><tr><td><strong>TRON network</strong></td><td>Mainnet / Nile</td><td>On-chain delegation &amp; settlement</td></tr></tbody></table>

{% hint style="warning" %}
Order fulfilment and delegation ultimately depend on the **TRON network** itself. If the underlying chain is congested or experiencing issues, orders may settle slowly even when TronSave's own services are fully operational.
{% endhint %}

See [Environments](../developers/environments.md) for full details on each environment.

## Reporting an incident

If you believe TronSave is down or degraded:

1. **Confirm the scope** — test the affected surface using the request above and note the HTTP status code or error message.
2. **Capture details** — record the timestamp, environment (Mainnet or Nile), affected endpoint or page, and any `requestId`/order ID returned.
3. **Report it** through an official support channel — Support/refunds: [t.me/wantingtrx](https://t.me/wantingtrx); Community: [t.me/tronsave](https://t.me/tronsave).

For non-outage problems (a stuck order, a failed transfer, an unexpected error), start with [Troubleshooting](troubleshooting.md) and [Transfer Recovery](transfer-recovery/README.md) before reporting an incident.

## Service level expectations

TronSave does not publish a formal, contractual Service Level Agreement (no uptime guarantee, support-response commitment, or service credits). Use of the service is governed by the [Terms of Service](terms-of-service.md).

{% hint style="info" %}
If you operate at scale and need uptime guarantees or a support response commitment, reach out via [t.me/wantingtrx](https://t.me/wantingtrx) to discuss enterprise terms.
{% endhint %}

## Building for resilience

Whether or not a formal SLA applies, treat the API as a remote dependency and design defensively:

* **Retry with backoff** on `5xx` and timeout responses; do not retry on `4xx`. See [Errors & Rate Limits](../developers/errors-and-rate-limits.md).
* **Make order creation idempotent** where possible so a retried request does not place a duplicate order.
* **Poll order status** rather than assuming a request succeeded — confirm `fulfilledPercent` before relying on the delegation.
* **Have a fallback** for time-critical transactions (e.g. let TRX burn) if an order cannot be filled in time.

## Next steps

* [Errors & Rate Limits](../developers/errors-and-rate-limits.md) — status codes and retry guidance
* [Troubleshooting](troubleshooting.md) — common problems and fixes
* [Environments](../developers/environments.md) — Mainnet and Nile endpoints
* [Terms of Service](terms-of-service.md)
