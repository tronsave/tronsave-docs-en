---
description: Buy Energy or Bandwidth with a TronSave API key and a prefunded internal account — no per-order on-chain signing required.
---

# Buy with API Key

The API-key flow lets you buy resources (Energy and Bandwidth) from a prefunded TronSave internal account. Instead of signing a transaction with your wallet for every order, you authenticate each request with an API key and pay from your internal account balance — so there is no per-order on-chain fee and integration stays simple.

{% hint style="info" %}
Prefer to pay per order directly from your wallet? Use the [Buy with Signed Transaction](../signed-tx/README.md) flow instead.
{% endhint %}

## Before you start

You need a TronSave API key and an internal account with enough balance to cover your orders. There are two ways to create an API key:

* **Option 1:** Generate the API key on the TronSave website.
* **Option 2:** Generate the API key on Telegram.

<!-- [NEEDS CONFIRMATION: relative doc paths for the "Get API Key — on the website" and "on Telegram" pages; the source linked to ../get-api-key/on-the-website and ../get-api-key/on-telegram which do not yet exist in the new content tree] -->

See [Authentication](../../../authentication.md) for how to pass the API key on each request.

## Endpoints

Use the API key to call any of the following endpoints:

<table>
<thead>
<tr><th>Endpoint</th><th>Description</th></tr>
</thead>
<tbody>
<tr><td><a href="get-account-info.md">Get Internal Account Info</a></td><td>Read your internal account balance and details.</td></tr>
<tr><td><a href="get-order-book.md">Get Order Book</a></td><td>Fetch the current order book for resource pricing and availability.</td></tr>
<tr><td><a href="estimate-trx.md">Estimate TRX</a></td><td>Calculate the TRX required for a resource amount and rental duration.</td></tr>
<tr><td><a href="create-order.md">Buy Energy (Create Order)</a></td><td>Place an order to buy Energy or Bandwidth, paid from your internal account.</td></tr>
<tr><td><a href="get-order-details.md">Get Order Details</a></td><td>Look up the details and status of a single order by ID.</td></tr>
<tr><td><a href="order-history.md">Get Internal Account Order History</a></td><td>List the order history for your internal account.</td></tr>
</tbody>
</table>

## Endpoints (TRON Nile Testnet)

{% tabs %}
{% tab title="Testnet" %}
* Estimate TRX: <mark style="color:orange;">`POST`</mark> `https://api-dev.tronsave.io/v2/estimate-buy-resource`
* Create order: <mark style="color:orange;">`POST`</mark> `https://api-dev.tronsave.io/v2/buy-resource`
* Get Internal Account Info: <mark style="color:blue;">`GET`</mark> `https://api-dev.tronsave.io/v2/user-info`
* Get Internal Account Order History: <mark style="color:blue;">`GET`</mark> `https://api-dev.tronsave.io/v2/orders`
* Get the order details: <mark style="color:blue;">`GET`</mark> `https://api-dev.tronsave.io/v2/order/:id`
* Get Order Book: <mark style="color:blue;">`GET`</mark> `https://api-dev.tronsave.io/v2/order-book`
{% endtab %}
{% endtabs %}

To integrate with the **Nile Testnet**, replace the base URL with [https://testnet.tronsave.io/](https://testnet.tronsave.io/) and follow the same steps.

## Next steps

* [Estimate TRX](estimate-trx.md)
* [Buy Energy (Create Order)](create-order.md)
* Learn the difference between [Energy and Bandwidth](../../../../concepts/energy-and-bandwidth.md).
* Review the [Order Types](../../../../concepts/order-types.md) before placing orders.
