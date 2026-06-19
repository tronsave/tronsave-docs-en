---
description: Place a standard market order on tronsave.io to rent Energy or Bandwidth that matches against current supply immediately.
---

# Normal Order

A Normal Order is the standard purchase on TronSave: you specify the amount, price and duration, and the order matches against current market supply right away. See [Order Types](../../../concepts/order-types.md) for how it compares to Pending, Smart, and other flows.

## Create an order

1. Open [tronsave.io/market](https://tronsave.io/market).
2. Connect your wallet (e.g. TronLink).
3. Enter the **Amount** of Energy/Bandwidth to buy, the **Price**, and the **Duration**. You can optionally set a custom resource target address. Fill in the form, then click **Create order**.

<figure><img src="../../../.gitbook/assets/market-create-order.png" alt="Market Create Order"><figcaption></figcaption></figure>

{% hint style="info" %}
Click the **Price** field to open the **Order Book** pop-up. From there you can check the available resources at different price levels and choose the most suitable price and quantity for your rental.
{% endhint %}

<figure><img src="../../../.gitbook/assets/market-order-book.png" alt="Market Order Book"><figcaption></figcaption></figure>

## Advanced settings

Open the **Settings** (⚙️) button to configure:

<table>
<thead>
<tr><th width="240">Setting</th><th>Description</th></tr>
</thead>
<tbody>
<tr>
<td><code>Minimum delegate</code> (Energy/Bandwidth)</td>
<td>The minimum amount of Energy/Bandwidth from a single provider that can be delegated to you. When set, the system matches your order only with providers whose available Energy/Bandwidth is equal to or greater than this value.</td>
</tr>
<tr>
<td><code>Allow partial fill</code></td>
<td>If checked, the order may be filled partially. If unchecked, the order will not be filled unless a single address can complete it in one transaction.</td>
</tr>
<tr>
<td><code>Immediate buy</code></td>
<td>If checked, the order must fill immediately. If the system is not ready to match it, the order is not created.</td>
</tr>
<tr>
<td><code>Priority payment</code></td>
<td>Preferred payment method. Default: always ask to confirm.</td>
</tr>
</tbody>
</table>

<figure><img src="../../../.gitbook/assets/market-advanced-settings.png" alt="Market Advanced Settings"><figcaption></figcaption></figure>

Once placed, the order appears on the **Orders** tab. If sufficient pool resource is available, it is filled automatically.

<figure><img src="../../../.gitbook/assets/order-history.png" alt="Order History"><figcaption></figcaption></figure>

## Update the target address

You can change an order's target (receiving) address if the order is **not fully matched** and **one hour has passed** since creation.

1. Open the **My Orders** tab and click into **Order Detail**.

<figure><img src="https://1055070949-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fh0rlDWTk05Qu3lvB5Gve%2Fuploads%2FMLYVevEpHpTOSIKBdC6u%2Fanh4.png?alt=media&#x26;token=0e0f1427-7968-4269-9a4d-c281ad5fda8d" alt=""><figcaption></figcaption></figure>

2. Click **Edit** and input the new address.

<figure><img src="https://1055070949-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fh0rlDWTk05Qu3lvB5Gve%2Fuploads%2FewVADk23iN7VgdeiACzO%2Fimage.png?alt=media&#x26;token=9a93c4bf-4647-441e-b271-8d57ee9a2c45" alt="" width="540"><figcaption></figcaption></figure>

3. Click **Confirm**.

<figure><img src="https://1055070949-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fh0rlDWTk05Qu3lvB5Gve%2Fuploads%2FTxo7WVa51MBeZ2oqAmGF%2Fimage.png?alt=media&#x26;token=a17f7e57-0c30-41b2-8d16-423c1539ed74" alt="" width="563"><figcaption></figcaption></figure>

## Update the price

1. Click the **Edit** button.

<figure><img src="https://1055070949-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fh0rlDWTk05Qu3lvB5Gve%2Fuploads%2FlGAHYwVVINHj63ofDw6j%2Fimage.png?alt=media&#x26;token=04604010-19a9-4024-be2b-00146b0790a7" alt="" width="550"><figcaption></figcaption></figure>

2. Input the new price and click **Confirm**.

<figure><img src="https://1055070949-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fh0rlDWTk05Qu3lvB5Gve%2Fuploads%2F1Nu3ii3mdP7SSQZ8tumc%2Fimage.png?alt=media&#x26;token=cf304c50-cf3e-4098-980f-7f44bd533eed" alt="" width="547"><figcaption></figcaption></figure>

## Cancel an order

You can cancel an order if it is **not matched within 5 minutes** of creation. The cancellation fee is **5 TRX**.

1. Click the **Cancel** button on your order.

<figure><img src="https://1055070949-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fh0rlDWTk05Qu3lvB5Gve%2Fuploads%2F55uRKdA3FDxnZd52g4aK%2Fimage.png?alt=media&#x26;token=96af31d5-51e8-447c-a7b5-5f9bfe8f64d8" alt=""><figcaption></figcaption></figure>

2. Click **Yes, I'm sure** to confirm the cancellation.

<figure><img src="https://1055070949-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fh0rlDWTk05Qu3lvB5Gve%2Fuploads%2FSSZWWOnFAD1EaKzxQQps%2Fimage.png?alt=media&#x26;token=be955dab-f223-414a-99ce-203c124697f7" alt=""><figcaption></figcaption></figure>

## Next steps

* [Order Types](../../../concepts/order-types.md) — Normal vs Pending, Smart, ZapBuy, and more
* [Pricing & APY](../../../concepts/pricing-and-apy.md)
* [Energy & Bandwidth](../../../concepts/energy-and-bandwidth.md)
