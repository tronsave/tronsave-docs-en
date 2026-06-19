---
description: Buy Energy for many TRON addresses in a single flow using a CSV upload.
---

# Bulk Buy Resource

Bulk Buy Resource purchases resources for multiple accounts at once. Instead of repeating the buy flow per wallet, you upload a list of addresses, set one amount and duration, and TronSave delegates Energy directly to every address.

It's built for anyone managing many wallets — bots, user accounts, or distribution setups — where buying one address at a time isn't practical.

## When to use it

* Buy Energy for **multiple addresses** in one pass instead of repeating the flow manually per wallet.
* Manage many accounts, bots, or user wallets from a single screen.
* Energy is sent directly to each address — no per-wallet signing loop.

{% hint style="info" %}
This tool complements the standard order flows. For everyday single-address buying, see [Buy Energy & Bandwidth](../buy/README.md).
{% endhint %}

## How to buy Energy for multiple addresses

### Step 1 — Open the tool

Go to [tronsave.io](https://tronsave.io/), open the **Tools** menu, and click [**Bulk Buy Resource**](https://tronsave.io/tools/multi-buy).

### Step 2 — Connect your wallet and fund your internal account

Make sure your TronSave internal account has enough balance to cover the bulk order. See [Get an API Key](../../developers/quickstart.md) for setup and deposit instructions.

### Step 3 — Import a CSV file

Upload a CSV file containing wallet addresses, one address per line.

<figure><img src="../../.gitbook/assets/tools-bulk-buy.png" alt="Tools Bulk Buy"><figcaption></figcaption></figure>

### Step 4 — Set amount and duration

* Choose the **Amount** `[1]` and **Duration** `[2]`.
* Then **get the API Key** `[3]`.

<figure><img src="https://1055070949-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fh0rlDWTk05Qu3lvB5Gve%2Fuploads%2FidxLFT7v5ohs7eHLdr4a%2Fimage.png?alt=media&#x26;token=0e62d050-89e2-4412-998d-27d909a7d8f6" alt=""><figcaption></figcaption></figure>

### Step 5 — Check and activate

Review the status of all addresses. If any wallet is not activated, you can activate it directly from this screen.

<figure><img src="https://1055070949-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fh0rlDWTk05Qu3lvB5Gve%2Fuploads%2F8xK870Dt7bRysoIrEvsJ%2Fimage.png?alt=media&#x26;token=a124cfa5-e9e1-4732-82f2-858531999868" alt=""><figcaption></figcaption></figure>

<figure><img src="https://1055070949-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fh0rlDWTk05Qu3lvB5Gve%2Fuploads%2FsTIkZaZxkxhfH0IoJRn9%2Fimage.png?alt=media&#x26;token=2b40074d-1684-41c0-ab7c-bbfab66cbb0d" alt=""><figcaption></figcaption></figure>

### Step 6 — Buy Energy

* Once every address shows the **Ready** status, click **Confirm** to proceed with the Energy purchase.

<figure><img src="https://1055070949-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fh0rlDWTk05Qu3lvB5Gve%2Fuploads%2FNn5Ya36rDEIcXBFMEqOI%2Fimage.png?alt=media&#x26;token=6220369b-6be0-4de3-9d2c-d7e179c1db02" alt=""><figcaption></figcaption></figure>

* A **Success** status means the order was created successfully. You can verify the transaction details on Tronscan for confirmation.

<figure><img src="https://1055070949-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fh0rlDWTk05Qu3lvB5Gve%2Fuploads%2FK6xWejsOfS9xvzNt7xxY%2Fimage.png?alt=media&#x26;token=f6203cc2-a1ef-427f-bf17-b3a35b0db178" alt=""><figcaption></figcaption></figure>

## Next steps

* [Bulk Send Token](bulk-send-token.md) — distribute TRX and TRC20 tokens to many recipients via CSV.
* [Transfer USDT](transfer-usdt.md) — cost-optimized USDT transfers.
* [Buy Energy & Bandwidth](../buy/README.md) · [Energy & Bandwidth](../../concepts/energy-and-bandwidth.md)
