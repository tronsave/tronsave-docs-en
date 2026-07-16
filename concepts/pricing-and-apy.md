---
description: How TRON Energy rental prices are set with unitPrice tiers, and how provider APY (around 18%) is calculated from rental income and voting rewards.
---

# Pricing & APY

## How buyers are priced

Rental price is set per order via `unitPrice` (SUN per resource unit) — either a fixed number or a tier (`SLOW` / `MEDIUM` / `FAST`). The market's live order book determines what a given tier resolves to. See [The Rental Model](rental-model.md#the-price-tiers) for the exact tier rules.

* **1 TRX = 1,000,000 SUN.**
* Always [estimate](/broken/pages/qSPR7nGa8LyjgLyYINRR) before ordering to see the current price and available supply.

## How provider APY is calculated

A provider's return combines the rental income (APR) compounded, plus TRON voting rewards (VR):

$$
APR_{tronsave} = \frac{AP \times FR}{1{,}000{,}000} \times PS \times 365
$$

<table><thead><tr><th width="234">Symbol</th><th>Meaning</th></tr></thead><tbody><tr><td><strong>AP</strong></td><td>Average Price of the 200 nearest matched orders (SUN)</td></tr><tr><td><strong>FR</strong></td><td>Freeze Rate on the TRON network (SUN per staked TRX)</td></tr><tr><td><strong>PS</strong></td><td>Profit Share of the provider on TronSave = <strong>0.75</strong></td></tr><tr><td>1,000,000</td><td>SUN per TRX</td></tr></tbody></table>

$$
APY_{total} = \left(1 + \frac{APR_{tronsave}}{12}\right)^{12} - 1 + VR
$$

* **VR** (Vote Rate): potential voting reward, \~**4%** — check at [tronscan.org](https://tronscan.org/#/sr/votes).

### Worked example

If **AP = 30 SUN**, **FR = 16.5**, **VR = 4%**:

$$
APR_{tronsave} = \frac{30 \times 16.5}{1{,}000{,}000} \times 0.75 \times 365 \approx 0.1355 = 13.55\%
$$

$$
APY_{total} = \left(1 + \frac{0.1355}{12}\right)^{12} - 1 + 4\% \approx 18.4\%
$$

{% hint style="info" %}
AP, FR and VR all move with market conditions, so live APY varies. Use the formula above with current values, or check the figure shown in the TronSave app.
{% endhint %}

## Next steps

* [Staking 2.0](staking-2.0.md) · [Sell / Provider](../guides/sell/) · [Calculate APY (FAQ)](../resources/faq.md)
