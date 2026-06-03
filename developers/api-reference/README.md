---
description: The TronSave REST API — base URLs, authentication, versioning, the response envelope, and links to every endpoint group.
---

# API Reference

The TronSave API lets you buy, extend, and sell Energy and Bandwidth on the TRON network programmatically. Every endpoint is a JSON REST call over HTTPS.

## Base URLs

| Environment | API base URL |
| --- | --- |
| Production | `https://api.tronsave.io` |
| Testnet (Nile) | `https://api-dev.tronsave.io` |

Endpoints are versioned by path prefix — the current version is `v2`, e.g. `https://api.tronsave.io/v2/buy-resource`. See [Environments & Networks](../environments.md) for the full list of hosts and chains.

## Authentication

Most write endpoints require an API key passed in the `apikey` request header:

```bash
curl -X POST https://api.tronsave.io/v2/buy-resource \
  -H "apikey: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{ ... }'
```

There are two ways to pay for and authorize an order:

* **Using an API key** — fund a TronSave internal account, then authenticate each request with your API key. Fast, easy to integrate, and saves transaction fees, but funds are held in a TronSave-managed internal account.
* **Using a signed transaction** — build a transaction, sign it with your private key, and submit it. Gives you full control of funds and a higher security bar, at the cost of on-chain transaction fees and a more complex integration.

See [Authentication](../authentication.md) for how to obtain a key, fund your account, and choose between the two methods.

## Versioning

`v2` is the current API and the one documented throughout this reference. The previous `v0` API is still available for existing integrations.

{% hint style="warning" %}
New integrations should use `v2`. The legacy `v0` API is maintained for backward compatibility only — see [Legacy API (v0)](../legacy-api-v0/README.md).
{% endhint %}

## Response envelope

Every API response is wrapped in a consistent JSON envelope:

```json
{
  "error": false,
  "message": "OK",
  "data": { }
}
```

<table>
  <thead>
    <tr><th width="160">Field</th><th>Description</th></tr>
  </thead>
  <tbody>
    <tr><td><code>error</code></td><td>Boolean flag — <code>false</code> on success, <code>true</code> on failure.</td></tr>
    <tr><td><code>message</code></td><td>Human-readable status or error message.</td></tr>
    <tr><td><code>data</code></td><td>The response payload (object) — the shape depends on the endpoint.</td></tr>
  </tbody>
</table>

For error codes, HTTP status mapping, and rate-limit behavior, see [Errors & Rate Limits](../errors-and-rate-limits.md).

## Endpoint groups

| Group | What it does |
| --- | --- |
| [Buy Resources](buy-resources/README.md) | Estimate cost and create orders to buy Energy or Bandwidth, via signed transaction or API key. |
| [Extend Orders](extend-orders/README.md) | Find extendable delegations and extend the duration of existing orders. |
| [Sell Resources](sell-resources.md) | List and manage resources you provide to the marketplace. |
| [Fast Charge](fast-charge/README.md) | Quickly top up resources — estimate, create, track, confirm, and cancel orders. |
| [MCP Server](mcp.md) | Use the TronSave Model Context Protocol server to drive the API from AI agents. |

## Next steps

* [Developer Quickstart](../quickstart.md) · [Authentication](../authentication.md) · [Environments & Networks](../environments.md)
* [Buy Resources](buy-resources/README.md) · [Errors & Rate Limits](../errors-and-rate-limits.md)
