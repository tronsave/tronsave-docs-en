---
description: >-
  Manually sell Energy or Bandwidth on TronSave by matching active buy orders
  with a signed on-chain delegate transaction.
---

# Sell Resources

The Sell Resources API lets a resource provider manually fill open buy orders on TronSave. The seller fetches active orders, builds and signs an on-chain `DelegateResourceContract` transaction, then submits the signed transaction to TronSave for order matching and payout.

{% hint style="warning" %}
**Whitelist required.** The Sell API is restricted to whitelisted wallets. Log in to TronSave and send your wallet address to support to add it to the whitelist. Once whitelisted, use the API Key generated from that wallet address to call the Sell endpoints.

Contact support on Telegram: [**@wantingtrx**](https://t.me/wantingtrx)
{% endhint %}

Selling is a two-step flow:

1. [**Get active orders**](get-active-orders.md) - list open buy orders you can fill.
2. [**Sell the resource**](sell-the-resources.md) - submit a signed delegate transaction against a chosen order.
3. [**Get Delegate Detail**](get-delegate-detail.md) - get detailed information and the current status of a specific delegate using its delegateId.

## Next steps

* New to the API? Start with the [Quickstart](../../quickstart.md) and [Authentication](../../authentication.md).
* Learn the difference between [Energy and Bandwidth](../../../concepts/energy-and-bandwidth.md) and review [Order Types](../../../concepts/order-types.md).
* Looking to buy instead of sell? See [Buy Resources](../buy-resources/).
