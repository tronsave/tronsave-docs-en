---
description: The TronSave rental flow end to end — from order to on-chain delegation.
---

# How It Works

TronSave is an **order‑book marketplace** for TRON resources. Buyers place orders for Energy/Bandwidth; providers supply it from their staked TRX; TronSave matches the two and performs the on‑chain **delegation**.

## The flow at a glance

```
Buyer ──places order──▶  TronSave order book  ◀──supplies resource── Provider
                              │
                              ├─ matches order at market price
                              ▼
                       On-chain delegation (Stake 2.0)
                              │
                              ▼
                   Receiver address gets Energy/Bandwidth
                        for the rental duration
```

1. **Estimate** — the buyer asks what an order will cost (`estimate-buy-resource`). TronSave returns a unit price (in [SUN](../concepts/glossary.md)) and the available supply.
2. **Order** — the buyer creates an order specifying `resourceType`, `resourceAmount`, `durationSec`, `receiver`, and a [price tier](../concepts/order-types.md) (`SLOW`/`MEDIUM`/`FAST` or a fixed SUN value).
3. **Match** — the order book matches the order against provider supply. Orders can fill fully, partially, or stay pending depending on supply and the order's options.
4. **Delegate** — matched resources are **delegated on‑chain** from the provider's account to the buyer's `receiver` address for the rental `durationSec`.
5. **Expire / Extend** — when the duration ends, the delegation is reclaimed. Buyers can [extend](../developers/api-reference/extend-orders/README.md) before expiry instead of re‑ordering.

## Two ways to pay

| | API Key (Internal Account) | Signed Transaction |
| --- | --- | --- |
| **How** | Deposit TRX into a TronSave internal account; costs are deducted automatically | Sign a TRX payment from your own wallet per purchase |
| **Best for** | Bots, backends, frequent buyers | Users who prefer not to hold TRX in TronSave |
| **Custody** | TronSave holds your deposited balance | You keep full custody |

See [Authentication](../developers/authentication.md) for both methods.

## Where it happens

* **Website** — [tronsave.io](https://tronsave.io) (full UI, all order types and tools)
* **Telegram** — the [TronSave bot](../guides/buy/on-telegram/README.md)
* **API / SDK** — [REST API](../developers/api-reference/README.md) and the [SDK](../developers/sdk.md) (TypeScript, Rust, Python, Java, PHP)

## Next steps

* [Quickstart](quickstart.md) · [Order Types](../concepts/order-types.md) · [The Rental Model](../concepts/rental-model.md)
