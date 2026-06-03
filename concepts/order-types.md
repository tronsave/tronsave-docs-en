---
description: Every way to buy on TronSave — Normal, Pending, Smart, ZapBuy, Auto Buy, Fast Charge.
---

# Order Types

TronSave offers several order types for different needs. This page is the conceptual overview; step‑by‑step guides live under [Buy Energy & Bandwidth](../guides/buy/README.md).

## Normal Order

The standard purchase: specify amount, duration and price, and the order matches against current market supply immediately. Best for everyday, on‑demand buying. → [Guide](../guides/buy/on-the-website/normal-order.md)

## Pending Order

The order is placed at your chosen price and **waits in the order book** until the market can match it. Best when you want a specific (often lower) price and aren't in a hurry. → [Guide](../guides/buy/on-the-website/pending-order.md)

## Smart Order

For **large** rentals that the market can't fully match at once. After the initial match, TronSave keeps monitoring the providers who partially filled your order; when they regain Energy (≥100,000), it **auto‑matches more** for the remaining duration and **refunds** any unused duration in TRX.

* Minimum **10,000,000 Energy**, minimum duration **3 days**.
* Active only during the **first one‑third** of the matched duration.
* Improves fill rate over time but does **not** guarantee 100% fulfillment. → [Guide](../guides/buy/on-the-website/smart-order.md)

## ZapBuy

The fastest path: **send TRX directly** to the TronSave bot address and a **1‑hour** Energy rental is created automatically for the sending wallet.

* Recipient bot address: `TLx8h8fjv5pyuxCu292ZgjbU14XZSiLGg4`
* Fixed **1‑hour** duration; **minimum 65,000 Energy** (less is ignored).
* Only fills if it can be **matched immediately**; send from a regular wallet (not a contract/exchange). → [Guide](../guides/buy/zapbuy.md)

## Auto Buy

Automatically tops up Energy for a watched address based on rules you set, so it never runs out. → [Guide](../guides/buy/auto-buy.md)

## Fast Charge (API)

A developer‑oriented flow for rapidly charging Energy with an estimate → create → track → confirm lifecycle. → [API Reference](../developers/api-reference/fast-charge/README.md)

## Quick comparison

| Type | Speed | Duration | Best for |
| --- | --- | --- | --- |
| Normal | Immediate | Custom | Everyday buying |
| Pending | Waits for price | Custom | Price‑sensitive buyers |
| Smart | Over time | ≥ 3 days | Large rentals (≥10M Energy) |
| ZapBuy | Instant | 1 hour (fixed) | Quick top‑ups via TRX transfer |
| Auto Buy | Automatic | Custom | Never‑run‑out automation |
| Fast Charge | Fast (API) | Custom | Programmatic charging |
