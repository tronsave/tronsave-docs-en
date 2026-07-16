---
description: How TronSave's order-book marketplace matches TRON Energy buyers with providers — price tiers, order fills, on-chain delegation, and duration.
---

# The Rental Model

TronSave is a **two‑sided marketplace** with an order book that matches **buyers** (who need Energy/Bandwidth) with **providers** (who have spare staked resources).

## Key roles

* **Buyer** — places an order to rent resources for a `receiver` address and a `durationSec`.
* **Provider (seller)** — delegates Energy from their staked TRX and earns [APY](pricing-and-apy.md).
* **Order book** — the live set of supply/demand that determines the market price.

## The price tiers

When buying, you set a `unitPrice` (in SUN per resource unit) — either a fixed number or one of three tiers:

<table><thead><tr><th width="181">Tier</th><th>Meaning</th></tr></thead><tbody><tr><td><code>SLOW</code></td><td>The lowest price at which the order can be set. Cheapest, may take longer / fill less.</td></tr><tr><td><code>MEDIUM</code></td><td>⭐ Default. The lowest price for the maximum market fill. If the market can't fill at all, <code>MEDIUM = SLOW + 10</code>.</td></tr><tr><td><code>FAST</code></td><td>Prioritizes immediate fill. If the market is 100% ready, <code>FAST = MEDIUM</code>; if &#x3C;100% ready, <code>FAST = MEDIUM + 10</code>; if 0%, <code>FAST = SLOW + 20</code>.</td></tr><tr><td><code>number</code></td><td>A fixed price in SUN for full control (e.g. <code>80</code>).</td></tr></tbody></table>

## How an order fills

Depending on supply and your [order options](order-types.md), an order can be:

* **Fully filled** — 100% matched and delegated.
* **Partially filled** — only possible with `allowPartialFill: true`.
* **Pending** — waiting for a provider to match (e.g., a [Pending Order](order-types.md)).

The status is reported as `fulfilledPercent` (0 = pending, 1–99 = partial, 100 = complete).

## Delegation & duration

Matched resources are **delegated on‑chain** from provider to the buyer's `receiver` for `durationSec`. When the duration ends, the delegation is reclaimed unless the order is [extended](../developers/api-reference/extend-orders/).

{% hint style="info" %}
Because matched duration can be shorter than requested (the provider's resources may unlock sooner), TronSave automatically **refunds the unused duration** in TRX — see [Smart Order](order-types.md#smart-order).
{% endhint %}

## Next steps

* [Order Types](order-types.md) · [Pricing & APY](pricing-and-apy.md) · [Staking 2.0](staking-2.0.md)
