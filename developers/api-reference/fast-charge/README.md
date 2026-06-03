---
description: Buy Energy for many addresses in one flow — estimate cost, create orders, track matching, then confirm to reclaim unused Energy and refund the difference.
---

# Fast Charge

Fast Charge lets you rent Energy for multiple addresses quickly and efficiently in a single flow, maximizing cost savings. You estimate the TRX you'll need, create the orders, track their matching status, and then confirm completed orders to reclaim unused Energy and receive a TRX refund.

{% hint style="info" %}
Fast Charge is an API-key feature — every call is authenticated against a prefunded TronSave internal account. See [Authentication](../../authentication.md) for how to obtain a key and fund your account.
{% endhint %}

## Lifecycle

The Fast Charge API follows a predictable lifecycle. Each step maps to one endpoint:

<table>
  <thead>
    <tr><th width="170">Step</th><th>Endpoint</th><th>What it does</th></tr>
  </thead>
  <tbody>
    <tr><td><strong>1. Estimate</strong></td><td><a href="get-an-estimate-trx.md">Get an Estimate TRX</a></td><td>Given your maximum acceptable price and the number of orders, estimate the TRX required. Check it against your internal account balance.</td></tr>
    <tr><td><strong>2. Create</strong></td><td><a href="create-fast-charge-orders.md">Create Fast Charge Orders</a></td><td>Submit the order with a max price, rental duration, and the list of addresses to receive Energy.</td></tr>
    <tr><td><strong>3. Track</strong></td><td><a href="tracking-fast-charge-order.md">Tracking Fast Charge Order</a></td><td>Monitor each order's status — <code>Pending</code> (waiting to match) or <code>Matched</code>.</td></tr>
    <tr><td><strong>4. Confirm</strong></td><td><a href="confirm-request.md">Confirm Request</a></td><td>After using the Energy, confirm orders to end the rental, reclaim unused Energy, and trigger the TRX refund.</td></tr>
    <tr><td><strong>Cancel</strong></td><td><a href="cancel-order.md">Cancel Order</a></td><td>Cancel a <code>Pending</code> order for a full TRX refund.</td></tr>
    <tr><td><strong>History</strong></td><td><a href="get-history.md">Get History</a></td><td>Look up the details of completed rents and orders.</td></tr>
  </tbody>
</table>

## Step 1 — Estimate TRX payout

Provide the rent details and let the system estimate the TRX you'll need:

* Maximum acceptable price.
* Number of orders to place.

The system returns the estimated TRX amount. Check your internal account balance to ensure it holds enough TRX for the rent before creating orders.

→ [Get an Estimate TRX](estimate-trx.md)

## Step 2 — Create Fast Charge orders

Submit an order with:

* Maximum acceptable price.
* Rental duration.
* List of addresses to receive the corresponding amount of Energy.

→ [Create Fast Charge Orders](create-order.md)

## Step 3 — Track order status

Use the tracking endpoint to monitor each order:

* **Matched** — the order has been matched.
* **Pending** — the order is waiting to be matched.

→ [Tracking Fast Charge Order](track-order.md)

## Step 4 — Confirm the order

After using the Energy, confirm the orders you want to complete by submitting the list for confirmation:

* Confirmation marks the end of the rental.
* The system reclaims unused Energy and calculates the TRX refund to return to your account.

{% hint style="success" %}
Confirming early helps you receive the **maximum possible refund**. If you never confirm, the system automatically reclaims the order after the rental period ends.
{% endhint %}

→ [Confirm Request](confirm-request.md)

## Cancel matched orders

To cancel an order, use the cancel endpoint:

* Only orders with **Pending** status can be canceled.
* Once canceled, 100% of the TRX is refunded to your internal account.

→ [Cancel Order](cancel-order.md)

## Check transaction history

Use the history endpoint to check the details of your completed rents and orders.

→ [Get History](get-history.md)

## Next steps

* [Get an Estimate TRX](estimate-trx.md) · [Create Fast Charge Orders](create-order.md) · [Tracking Fast Charge Order](track-order.md)
* [Authentication](../../authentication.md) · [Errors & Rate Limits](../../errors-and-rate-limits.md)
