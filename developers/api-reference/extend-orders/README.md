---
description: Extend an existing TronSave order (v2) in two steps — fetch extendable delegates, then submit an extend request paid by API key or signed transaction.
---

# Extend Orders

The v2 extend endpoints let you prolong an order you already hold instead of placing a new one. Version 2 adds two improvements over earlier extend support:

* Extension via **signed transactions**, not just an API key.
* A new resource type: **Bandwidth** (in addition to **Energy**).

Extending is a two-step flow: first read which delegates on an order can be extended, then submit an extend request for the ones you want.

## The two-step flow

| Step | What it does | Page |
| --- | --- | --- |
| 1 | List the delegates on your order that are eligible for extension | [Get extendable delegates](get-extendable-delegates.md) |
| 2 | Submit the extension and pay for it | [Submit extend request](extend-request.md) |

### Step 1: Get extendable delegates

Query an order to find out which delegations can still be extended and for how long. Use the result to build the payload for step 2.

→ [Get extendable delegates](get-extendable-delegates.md)

### Step 2: Submit extend request

Send the extend request for the chosen delegates. Two payment methods are supported:

* **Option 1 — Extend using an API key:** authorize the extension with a TronSave API key on a prefunded internal account; no per-order on-chain signing.
* **Option 2 — Extend using a signed transaction:** sign the extension with your own private key and settle it on-chain.

→ [Submit extend request](extend-request.md)

## Endpoints

The extend flow uses two `POST` endpoints:

| Step | Method | Path |
| --- | --- | --- |
| Get extendable delegates | `POST` | `/v2/get-extendable-delegates` |
| Submit extend request | `POST` | `/v2/extend-request` |

{% hint style="info" %}
A Postman collection for the extend flow is available at [postman.com/tronsave/tronsave](https://www.postman.com/tronsave/tronsave/folder/01yhu2j/extend-order).
{% endhint %}

## Testing on TRON Nile Testnet

Use the development host to test against the Nile Testnet:

{% tabs %}
{% tab title="Testnet" %}
* Get extendable delegates: `POST` `https://api-dev.tronsave.io/v2/get-extendable-delegates`
* Submit extend request: `POST` `https://api-dev.tronsave.io/v2/extend-request`
{% endtab %}
{% endtabs %}

To integrate with the Nile Testnet from the website, replace the host with [https://testnet.tronsave.io/](https://testnet.tronsave.io/) and follow the same steps as the [Get API Key](../../authentication.md) guide.

## Next steps

* [Get extendable delegates](get-extendable-delegates.md) — start the flow.
* Set up [Authentication](../../authentication.md) before calling the API.
* Review [Energy and Bandwidth](../../../concepts/energy-and-bandwidth.md) and [Order Types](../../../concepts/order-types.md).
