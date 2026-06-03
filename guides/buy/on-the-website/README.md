---
description: Buy Energy and Bandwidth from the TronSave website — connect a wallet, then place a Normal, Pending, or Smart order.
---

# Buy on the Website

The TronSave website at [tronsave.io](https://tronsave.io) provides the full buying UI with every order type and tool. This is the no-code path: connect a TRON wallet, choose what to rent, and sign.

## Before you start

* A TRON wallet (e.g. TronLink) connected to the site via **Connect**.
* The [receiver address](../../../concepts/glossary.md) that should get the Energy or Bandwidth — by default this is your connected wallet.

{% hint style="info" %}
New to the marketplace? Read [How It Works](../../../getting-started/how-it-works.md) and the [Quickstart](../../../getting-started/quickstart.md) first.
{% endhint %}

## The basic flow

1. Go to [tronsave.io/market](https://tronsave.io/market) and click **Connect** to connect your wallet.
2. Choose **Buy**, then set the order parameters (resource, amount, duration, price).
3. Pick an order type — Normal, Pending, or Smart (see below).
4. Confirm and sign the transaction. Once the order matches, the resource is delegated to your receiver address on-chain.

## Choose an order type

The website supports three order types. Each has its own step-by-step guide:

<table>
  <thead>
    <tr><th>Order type</th><th>Best for</th><th>Guide</th></tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Normal Order</strong></td>
      <td>Everyday, on-demand buying — matches against current supply immediately.</td>
      <td><a href="normal-order.md">Normal Order</a></td>
    </tr>
    <tr>
      <td><strong>Pending Order</strong></td>
      <td>Price-sensitive buyers — waits in the order book until the market matches your price.</td>
      <td><a href="pending-order.md">Pending Order</a></td>
    </tr>
    <tr>
      <td><strong>Smart Order</strong></td>
      <td>Large rentals (≥ 10M Energy, ≥ 3 days) the market can't fully match at once.</td>
      <td><a href="smart-order.md">Smart Order</a></td>
    </tr>
  </tbody>
</table>

For the conceptual differences between all order types, see [Order Types](../../../concepts/order-types.md).

## Other ways to buy

* **Telegram** — the [TronSave bot](../on-telegram/README.md)
* **API / SDK** — [REST API](../../../developers/api-reference/README.md) and the [SDK](../../../developers/sdk.md) (TypeScript, Rust, Python, Java, PHP)

## Next steps

* [Normal Order](normal-order.md) · [Pending Order](pending-order.md) · [Smart Order](smart-order.md)
* [Order Types](../../../concepts/order-types.md) · [How It Works](../../../getting-started/how-it-works.md)
