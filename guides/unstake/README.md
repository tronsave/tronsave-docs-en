---
description: Withdraw staked TRX early on TronSave without waiting for the official unstake period.
---

# Unstake

Unstaking on TRON normally locks your TRX for the official unstake period before you can withdraw it. TronSave's **Early Unstake** lets you get faster access to that liquidity, with a variable service fee that depends on the unstake amount.

There are two ways to handle an early unstake on TronSave:

<table>
<thead>
<tr><th>Method</th><th>What it does</th></tr>
</thead>
<tbody>
<tr><td><a href="early-unstake.md">Early Unstake</a></td><td>Instantly withdraw your staked TRX. If the system has enough liquidity your request is auto-approved; otherwise it goes to manual review.</td></tr>
<tr><td><a href="unstake-market.md">Unstake Market</a></td><td>List your unstake order on the marketplace for manual matching by providers when liquidity isn't immediately available.</td></tr>
</tbody>
</table>

{% hint style="info" %}
All Early Unstake transactions are fully verifiable on the TRON blockchain. Always verify the official TronSave address before granting wallet permissions — for higher safety, contact TronSave support first to confirm authenticity.
{% endhint %}

## How liquidity affects your request

When you submit an unstake request, TronSave checks it against current system liquidity:

* **Sufficient liquidity** — your request is automatically approved and processed.
* **Insufficient liquidity** — auto-approval is liquidity-based with no fixed threshold, so your request is sent to the admin team and providers for manual review and payout approval.
* **Liquidity still unavailable after 24 hours** — your unstake order is listed on the [Unstake Market](unstake-market.md) for manual matching by providers.

## Need help?

TronSave commits to processing all Early Unstake requests as quickly as possible, within available liquidity limits. If your request requires manual approval or runs into an issue, reach the official support channel:

* Telegram Support: [@wantingtrx](https://t.me/wantingtrx)

## Next steps

* [Early Unstake](register-unstake.md)
* [Unstake Market](unstake-market.md)
