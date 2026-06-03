---
description: End-to-end, copy-paste code examples for buying and extending Energy and Bandwidth through the TronSave API.
---

# Code Examples

These pages contain complete, runnable scripts that walk through a full TronSave integration end to end — from building and signing a transaction to placing an order and confirming it was filled. Each example is self-contained: copy it, set your credentials, and run it.

The examples come in two flavors:

* **API Key** — authenticate with a TronSave API Key. Best for backend services that pay from a prepaid TronSave balance. See [Authentication](../authentication.md).
* **Private Key** — sign and pay directly from a TRON account using its private key, no prepaid balance required.

{% hint style="info" %}
All examples use [TronWeb](https://tronweb.network/) `5.3.2`. Install it with:

```bash
npm i tronweb@5.3.2 @noble/secp256k1@1.7.1
```

Read more in the [TronWeb 5.3.2 release notes](https://tronweb.network/docu/docs/5.3.2/Release%20Note/).
{% endhint %}

{% hint style="warning" %}
Most examples reference the mainnet TronSave receiver address `TWZEhq5JuUVvGtutNgnRBATbF8BnHGyn4S`. When running against testnet (Nile), change it to `TATT1UzHRikft98bRFqApFTsaSw73ycfoS`. See [Environments & Networks](../environments.md).
{% endhint %}

## v2 (current)

Use these examples for new integrations. They target the current TronSave API.

<table data-header-hidden><thead><tr><th></th><th></th><th></th></tr></thead><tbody>
<tr><td><strong>Example</strong></td><td><strong>Auth</strong></td><td><strong>Description</strong></td></tr>
<tr><td><a href="v2/buy-with-api-key.md">Buy Resources with an API Key</a></td><td>API Key</td><td>Estimate cost and place a resource order paying from your TronSave balance.</td></tr>
<tr><td><a href="v2/buy-with-private-key.md">Buy Resources with a Private Key</a></td><td>Private Key</td><td>Sign and pay for a resource order directly from a TRON account.</td></tr>
<tr><td><a href="v2/extend-with-api-key.md">Extend an Order with an API Key</a></td><td>API Key</td><td>Extend an active delegation, paying from your TronSave balance.</td></tr>
<tr><td><a href="v2/extend-with-private-key.md">Extend an Order with a Private Key</a></td><td>Private Key</td><td>Extend an active delegation, signing and paying from a TRON account.</td></tr>
</tbody></table>

## v0 (legacy)

{% hint style="warning" %}
These are legacy v0 examples, kept for reference only. For new integrations use the v2 examples above.
{% endhint %}

<table data-header-hidden><thead><tr><th></th><th></th><th></th></tr></thead><tbody>
<tr><td><strong>Example</strong></td><td><strong>Auth</strong></td><td><strong>Description</strong></td></tr>
<tr><td><a href="v0/buy-with-api-key.md">Buy Energy with an API Key</a></td><td>API Key</td><td>Legacy flow for buying Energy paying from your TronSave balance.</td></tr>
<tr><td><a href="v0/buy-with-private-key.md">Buy Energy with a Private Key</a></td><td>Private Key</td><td>Legacy flow for buying Energy signing from a TRON account.</td></tr>
<tr><td><a href="v0/extend-with-api-key.md">Extend an Order with an API Key</a></td><td>API Key</td><td>Legacy flow for extending an active delegation with an API Key.</td></tr>
</tbody></table>

## Next steps

* [Quickstart](../quickstart.md) — the shortest path to your first order.
* [API Reference](../api-reference/README.md) — every endpoint these examples call.
* [SDK](../sdk/README.md) — a typed TypeScript wrapper if you prefer not to call the HTTP API directly.
