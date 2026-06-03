---
description: >-
  TronSave's two API environments — Mainnet and the Nile testnet — and how to
  switch between them.
---

# Environments & Networks

TronSave runs on two networks: **Mainnet** for real orders, and the **Nile testnet** for development and testing. The API surface is identical across both — only the base domain changes.

## Mainnet vs. Nile testnet

<table><thead><tr><th width="124"></th><th>Mainnet (Production)</th><th>Nile (Testnet)</th></tr></thead><tbody><tr><td><strong>Website</strong></td><td><code>https://tronsave.io</code></td><td><code>https://testnet.tronsave.io</code></td></tr><tr><td><strong>API base</strong></td><td><code>https://api.tronsave.io</code></td><td><code>https://api-dev.tronsave.io</code></td></tr><tr><td><strong>TRON network</strong></td><td>Mainnet</td><td>Nile testnet</td></tr><tr><td><strong>TRX</strong></td><td>Real TRX</td><td>Test TRX (no real value)</td></tr></tbody></table>

{% hint style="info" %}
**All endpoint paths are identical** across environments. To target the testnet, swap only the base domain — for example **`https://api.tronsave.io/v2/user-info`** becomes \
&#xNAN;**`https://api-dev.tronsave.io/v2/user-info`**.
{% endhint %}

## Switching environments

Keep the base URL in a single variable so you can move between environments by changing one value.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Mainnet
const TRONSAVE_API = "https://api.tronsave.io";

// Nile testnet
// const TRONSAVE_API = "https://api-dev.tronsave.io";

const res = await fetch(`${TRONSAVE_API}/v2/user-info`, {
  headers: { apikey: "YOUR_API_KEY" },
});
```
{% endtab %}

{% tab title="cURL" %}
```bash
# Mainnet
curl https://api.tronsave.io/v2/user-info -H "apikey: YOUR_API_KEY"

# Nile testnet
curl https://api-dev.tronsave.io/v2/user-info -H "apikey: YOUR_API_KEY"
```
{% endtab %}
{% endtabs %}

## Testing on Nile

The Nile testnet behaves exactly like Mainnet but uses test TRX, so you can run the full estimate → buy → check-status flow without spending real funds.

1. Go to [testnet.tronsave.io](https://testnet.tronsave.io) and connect a wallet configured for the Nile network.
2. Get an API key and deposit address the same way you would on Mainnet — see [Quickstart](quickstart.md).
3. Fund your account with test TRX from the [Nile faucet](https://nileex.io/join/getJoinPage).

{% hint style="warning" %}
API keys, internal account balances, and orders are **separate** between Mainnet and Nile. An API key issued on one environment does not work on the other.
{% endhint %}

## Shasta is not supported

TronSave does **not** support the **Shasta** testnet. Use the **Nile** testnet for all testing.

## Next steps

* [Quickstart](quickstart.md) · [Authentication](authentication.md)
* [Glossary](../concepts/glossary.md) · [Order Types](../concepts/order-types.md)
