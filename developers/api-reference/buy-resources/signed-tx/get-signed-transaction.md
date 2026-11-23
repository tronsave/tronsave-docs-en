---
description: >-
  Generate a signed TRX payment transaction for a resource order — either with
  your own code or via the TronSave Get Signed Transaction API.
---

# Get Signed Transaction

This is **Step 2** of the [Buy with Signed Transaction](./) flow. After [Step 1 — Estimate TRX](estimate-trx.md) returns an `estimateTrx` (the cost of the buy order in SUN), you produce a signed transfer transaction that pays that amount from the buyer's wallet to the TronSave fund address. You then submit it in [Step 3 — Create Order](create-order.md).

You can do this two ways: sign the transaction yourself ([Option 1](get-signed-transaction.md#option-1-write-your-own-function)), or let the TronSave API sign it for you ([Option 2](get-signed-transaction.md#option-2-use-the-tronsave-api)).

## Write your own function

Using the `estimateTrx` value from [Step 1](estimate-trx.md), build a TRX transfer transaction with `transactionBuilder` that sends an amount equal to `estimateTrx` from the buyer's address to the TronSave fund address, then sign it with the buyer's private key:

```tsx
const dataSendTrx = await tronWeb.transactionBuilder.sendTrx('TRONSAVE_FUND_ADDRESS', estimate_trx, 'BUYER_ADDRESS')
const signed_tx = await tronWeb.trx.sign(dataSendTrx, 'PRIVATE_KEY');
```

{% hint style="info" %}
* `BUYER_ADDRESS` — the buyer's public address.
* `PRIVATE_KEY` — the buyer's private key.
* `TRONSAVE_FUND_ADDRESS` — the TronSave fund address (see below).
{% endhint %}

**TronSave fund address (`TRONSAVE_FUND_ADDRESS`):**

{% tabs %}
{% tab title="MAINNET" %}
```
TWZEhq5JuUVvGtutNgnRBATbF8BnHGyn4S
```
{% endtab %}

{% tab title="Nile (for testing)" %}
{% hint style="warning" %}
This fund address is for testing purposes only on the TRON Nile network.
{% endhint %}

```
TATT1UzHRikft98bRFqApFTsaSw73ycfoS
```
{% endtab %}
{% endtabs %}

The result `signed_tx` is then passed to [Step 3 — Create Order](create-order.md).

## Next steps

* [Step 1 — Estimate TRX](estimate-trx.md)
* [Step 3 — Create Order](create-order.md)
* [Buy with Signed Transaction overview](./)
