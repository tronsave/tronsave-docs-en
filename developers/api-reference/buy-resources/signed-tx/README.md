---
description: Buy Energy or Bandwidth by submitting a signed TRON transaction — estimate cost, get a signed transaction, then create the order.
---

# Buy with Signed Transaction

The signed-transaction flow lets you buy resources by signing a TRON transaction with your own wallet key. Instead of holding a TronSave balance, you pay per order directly from your wallet. The flow has three API steps:

1. **Estimate TRX** — calculate the TRX required for the resource amount and rental duration.
2. **Get a signed transaction** — produce a signed transaction, either with your own code or via the TronSave API.
3. **Create an order** — submit the signed transaction to finalize the resource purchase.

{% hint style="info" %}
Prefer not to sign transactions yourself? You can also buy with a prepaid balance using your API key. See [Buy with API Key](../api-key/README.md).
{% endhint %}

## Before you start

1. Have access to the private key of the wallet you intend to use for the transaction.
2. Make sure the wallet holds enough TRX to pay for the order.

## The flow

### Step 1 — Estimate TRX required

Call the [Estimate TRX](estimate-trx.md) endpoint to calculate the TRX needed for the desired resource amount and rental duration.

### Step 2 — Generate a signed transaction

You have two options:

* **Option 1:** [Write your own function](get-signed-transaction.md) to generate the signed transaction using the wallet's private key.
* **Option 2:** Use the TronSave [Get Signed Transaction](get-signed-transaction.md) API to generate the signed transaction automatically.

### Step 3 — Create an order

Call the [Create Order](create-order.md) endpoint to finalize the purchase. Pass:

* The signed transaction (from the API response, or your own custom-signed transaction).
* The estimated TRX amount and other required parameters.

## Endpoints (TRON Nile Testnet)

{% tabs %}
{% tab title="Testnet" %}
* Estimate TRX: <mark style="color:orange;">`POST`</mark> `https://api-dev.tronsave.io/v2/estimate-buy-resource`
* Get Signed Transaction: <mark style="color:orange;">`POST`</mark> `https://api-dev.tronsave.io/v2/signed-tx`
* Create Order: <mark style="color:orange;">`POST`</mark> `https://api-dev.tronsave.io/v2/buy-resource`
{% endtab %}
{% endtabs %}

To integrate with the **Nile Testnet**, replace the base URL with [https://testnet.tronsave.io/](https://testnet.tronsave.io/) and follow the same steps.

{% hint style="info" %}
A ready-to-use Postman collection for this flow is available: [Buy with Signed Transaction (Postman)](https://www.postman.com/tronsave/tronsave/folder/kuv179r/buy-with-signtx).
{% endhint %}

## Next steps

* [Estimate TRX](estimate-trx.md)
* [Get Signed Transaction](get-signed-transaction.md)
* [Create Order](create-order.md)
