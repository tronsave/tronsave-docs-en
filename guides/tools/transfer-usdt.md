---
description: Send USDT on TRON with automatic Energy estimation, optional Energy purchase, and recipient fraud checks.
---

# Transfer USDT

The **Transfer USDT** tool sends USDT (TRC‑20) to any TRON address from a single screen. Before broadcasting it estimates the [Energy](../../concepts/energy-and-bandwidth.md) the transfer needs, offers to buy that Energy from the TronSave market to avoid burning TRX, and warns you if the recipient looks like a fraud or spam address.

Open it at [tronsave.io/tools/transfer-token](https://tronsave.io/tools/transfer-token) (under the **Tools** menu).

***

## Key features

### One-click USDT transfer

Send USDT in a single transaction to any TRON address. The interface is built for quick payments — enter a receiver and an amount, review, and confirm.

<figure><img src="https://1055070949-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fh0rlDWTk05Qu3lvB5Gve%2Fuploads%2FEj4qjxR3IFi886yM0BEX%2Fimage.png?alt=media&#x26;token=3265aa71-29da-474a-b940-389ec24ab82d" alt=""><figcaption></figcaption></figure>

### Energy estimation and optimization

The tool automatically estimates the Energy required for the transfer. If your wallet does not have enough Energy, it will:

1. Prompt you that Energy is insufficient.
2. Offer an **auto-purchase option** from the TronSave Energy market.

Renting the Energy instead of letting the network burn TRX lowers the overall cost of the transfer.

{% hint style="info" %}
A USDT TRC‑20 transfer consumes Energy. Without enough Energy, TRON burns TRX to cover the difference, which is usually far more expensive than renting. See [Energy & Bandwidth](../../concepts/energy-and-bandwidth.md).
{% endhint %}

### Fraud and spam address detection

Before sending, the tool checks the receiver address against a fraud/spam database. If the address is flagged as potentially malicious, you see a warning message, reducing the risk of sending USDT to an unsafe wallet.

The check uses the [TRONSCAN security service API](https://docs.tronscan.org/security-service/security-service-api#check-account-security).

***

## User flow

1. Go to [tronsave.io](https://tronsave.io/), open the **Tools** menu, and click [Transfer USDT](https://tronsave.io/tools/transfer-token).
2. Enter the **receiver address** and **amount**.
3. **System check** — the tool estimates the required Energy and verifies whether the recipient is flagged as potential fraud/spam.
4. **Energy handling** — if Energy is insufficient, the tool shows a **Buy Energy** option backed by the TronSave market.
5. **Transfer** — review the details and confirm.

***

## Benefits

* Simple and secure USDT transfers.
* Minimized TRX cost via on-demand Energy purchase.
* Fraud prevention with real-time recipient warnings.

***

## Next steps

* [Energy & Bandwidth](../../concepts/energy-and-bandwidth.md) — why USDT transfers need Energy.
* [Glossary](../../concepts/glossary.md) — terminology used across the docs.
