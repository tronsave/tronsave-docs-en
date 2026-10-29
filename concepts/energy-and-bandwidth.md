---
description: >-
  The two resources every TRON transaction consumes — and why renting them saves
  money.
---

# Energy & Bandwidth

Every action on the TRON network consumes two resources. Understanding them is the key to understanding why TronSave saves you money.

👉 **[Rent TRON energy on the TronSave market](https://tronsave.io/market)** — live prices for any amount and duration; the energy is delegated to your wallet, no staking needed.

## Energy

* **What it's for:** executing **smart contracts** — e.g. transferring USDT (a TRC‑20 token), interacting with a DEX, minting, etc.
* **How you get it:** by **staking TRX** (Stake 2.0), or by **renting** it on TronSave.
* **If you don't have enough:** the network **burns TRX** to cover the shortfall — this is what makes contract calls expensive.

> A single **USDT TRC‑20 transfer** needs about **64,285 Energy** when the receiver already holds USDT, and about **130,285 Energy** when it does not. (Approximate: the USDT contract's energy factor changes over time.)
>
> Without Energy, TRON burns **100 SUN per Energy unit** — about **6.4 TRX** per transfer to a wallet that holds USDT, about **13 TRX** to one that does not. Check your own numbers with the [TRON fee calculator](https://tronsave.io/tools/calculator).

## Bandwidth

* **What it's for:** covering the **byte size** of a transaction. Every transaction needs Bandwidth.
* **How you get it:** a small **free daily allowance** per account, plus staking TRX or renting.
* **If you don't have enough:** the network burns a small amount of TRX.

## Why rent instead of burn?

When you lack staked resources, TRON burns TRX at the protocol rate. On TronSave you instead pay the **market rental rate**, which is typically **far cheaper** — up to **\~70%** savings for frequent operations like USDT transfers.

|            | Burn TRX (no resources) | Rent on TronSave                      |
| ---------- | ----------------------- | ------------------------------------- |
| Cost basis | Protocol burn rate      | Market rate (often much lower)        |
| Setup      | None                    | Place an order / hold a small balance |
| Best for   | One‑off, tiny usage     | Frequent transfers, bots, dApps       |
| One USDT transfer (receiver holds USDT) | \~6.4 TRX burned | Live quote on the [TRON energy market](https://tronsave.io/market) |

## Units

* Resources are counted in **Energy** / **Bandwidth** units.
* Prices are quoted in **SUN** per unit, where **1 TRX = 1,000,000 SUN**.
* For rentals **shorter than one day**, the price covers the whole rental. For **one day or longer**, it is a price **per day**: total = amount × price × number of days.

See the [Glossary](glossary.md) for all terms, and [Pricing & APY](pricing-and-apy.md) for how rental prices are set.

## Next steps

* [Rent TRON energy](https://tronsave.io/market) · [Fee calculator](https://tronsave.io/tools/calculator)
* [The Rental Model](rental-model.md) · [Order Types](order-types.md) · [Quickstart](../getting-started/quickstart.md)
