---
description: Common questions about pricing, Energy & Bandwidth, and buying resources on TronSave.
---

# FAQ

Answers to the questions we hear most often. For deeper background, see [Energy & Bandwidth](../concepts/energy-and-bandwidth.md) and the [Glossary](../concepts/glossary.md).

## Why is the price higher when buying with the bot than on the website?

There are two common explanations.

**Case 1 — Target address mismatch.** Double-check the **target address** in your order. Discrepancies in the target address can lead to price differences.

**Case 2 — Different matching strategy.** The bot and the website use different purchasing flows:

* On the **website**, you can set the lowest possible price — which means the order may not match 100%.
* On the **bot**, orders are always placed at the **`MEDIUM`** price to ensure every bot purchase matches 100%.

In most cases the bot and website prices are the same. The bot's price is only higher than the website's when the system runs out of resources to match against.

{% hint style="info" %}
If you want full control over price (and accept the chance of a partial fill), buy on the website. If you want a guaranteed 100% match, use the bot.
{% endhint %}

## What are TRON Energy and Bandwidth?

Energy and Bandwidth are the two resources every TRON transaction consumes. They are acquired by **locking (staking) TRX**, which generates these resources during the lock period. While your TRX is frozen, it cannot be traded, bought, or sold until it is unfrozen after a specified number of days.

* **Bandwidth** is granted as a reward for freezing TRX and is required for the byte size of a transaction.
* **Energy** is an exclusive TRON resource representing a unit of CPU consumption within the network. It can only be obtained by freezing TRX.

Both resources are essential for creating and executing smart contracts. See [Energy & Bandwidth](../concepts/energy-and-bandwidth.md) for full detail.

## How do I get Energy?

There are three ways to obtain Energy.

### Method 1 — Burn TRX

If your account does not have enough Energy, TRX is **automatically burned** during a transfer to cover the Bandwidth and Energy the transfer requires. Compared with consuming staked Energy, burning TRX is **not cost-effective**.

### Method 2 — Freeze (stake) TRX

Open the TRON resource management interface ([https://tronscan.io/#/wallet/resources](https://tronscan.io/#/wallet/resources)), choose the resource type you want, and enter the amount of TRX to freeze.

The Energy you obtain is calculated as:

```
Energy obtained = (frozen TRX / total network frozen TRX) × total network Energy limit
```

This distributes the fixed network Energy evenly across all frozen TRX. Frozen TRX can be thawed and retrieved after **14 days**. Consider this method if you have a lot of idle TRX.

{% hint style="warning" %}
Staking locks your TRX for 14 days before it can be unfrozen and reclaimed. Plan accordingly if you need liquidity.
{% endhint %}

### Method 3 — Buy Energy on TronSave

Renting Energy through [TronSave](https://tronsave.io) saves on fees. You don't need to freeze a large amount of TRX or burn extra TRX, and you can save up to **92%** of the handling fee.

See [Order Types](../concepts/order-types.md) and the [Quickstart](../getting-started/quickstart.md) to get started.

## Next steps

* [Energy & Bandwidth](../concepts/energy-and-bandwidth.md) · [Order Types](../concepts/order-types.md) · [Glossary](../concepts/glossary.md)
