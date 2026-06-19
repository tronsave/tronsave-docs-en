---
description: Configure TronSave to automatically match and fill buyer orders with your staked Energy or Bandwidth, on your own price and duration rules.
---

# Auto Sell

**Auto Sell** lets TronSave match and fill incoming buyer orders with your staked Energy or Bandwidth automatically. You set your price floor, duration limits, and profit share once, grant the system delegation permission, and TronSave handles matching, delegating, and paying out for you.

{% hint style="info" %}
To sell, you first need staked resources to delegate. If you only hold TRX, stake it first — see [Get Energy by Staking 2.0](staking-2.0.md). For the underlying mechanics, see the [Staking 2.0](../../concepts/staking-2.0.md) and [Order Types](../../concepts/order-types.md) concepts.
{% endhint %}

## Configure Auto Sell

### Step 1: Open the Seller tab in the TronSave market

Go to the [Seller settings page](https://tronsave.io/dashboard/seller/settings).

### Step 2: Connect your TRON wallet and log in

* Open the [TronSave market](https://tronsave.io/dashboard/seller/settings) and connect your wallet. <!-- [NEEDS CONFIRMATION: wallet connection guide path — source linked to ../../faq/how-to-connect-wallet-in-tronsave] -->
* Click **Login TRONSAVE** and sign the message to log in.

<figure><img src="../../.gitbook/assets/seller-desk.png" alt="Seller Desk"><figcaption></figcaption></figure>

### Step 3: Stake Energy/Bandwidth (optional)

_If you already have available resources, you can skip this step._

If you only hold TRX and haven't staked Energy or Bandwidth yet, stake before selling. You can do this in either of the following ways:

1. **Stake via TronScan** ([stake link](https://tronscan.org/#/wallet/resources)).
2. **Stake directly on TronSave** — click **Stake more**.

<figure><img src="https://1055070949-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fh0rlDWTk05Qu3lvB5Gve%2Fuploads%2FfwUqlD1qDOSrUTfZTtw8%2Fimage.png?alt=media&#x26;token=838f5014-e16a-4ce9-b60d-c53e2253dc01" alt="" width="563"><figcaption></figcaption></figure>

Choose **Energy** or **Bandwidth**, enter the **Staking Amount**, then click **Stake**.

<figure><img src="https://1055070949-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fh0rlDWTk05Qu3lvB5Gve%2Fuploads%2FQjYD2RABu1udoqXDWCEB%2Fimage.png?alt=media&#x26;token=9358a078-4e83-4e09-8834-b98ff51af4c8" alt="" width="560"><figcaption></figcaption></figure>

### Step 4: Grant delegation permission to TronSave

Auto Sell needs permission to delegate resources from your account on your behalf. <!-- [NEEDS CONFIRMATION: permission guide page path — source linked to ../permission] -->

### Step 5: Edit your Auto Sell conditions

Set the matching rules to fit your strategy.

<figure><img src="../../.gitbook/assets/seller-automations.png" alt="Seller Automations"><figcaption></figcaption></figure>

<table>
  <thead>
    <tr><th>#</th><th>Setting</th><th>Description</th></tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td><code>Automatic matching</code></td>
      <td>Automatically match with orders that meet your criteria.</td>
    </tr>
    <tr>
      <td>2</td>
      <td><code>Earning Share</code></td>
      <td>The % profit you want to take. A higher share lowers your order's matching priority.</td>
    </tr>
    <tr>
      <td>3</td>
      <td><code>Allow "Extend Order"</code></td>
      <td>Allow the buyer to create an extended order.</td>
    </tr>
    <tr>
      <td>4</td>
      <td><code>Max Delegate Duration</code></td>
      <td>The maximum delegation duration. Default is 30 days.</td>
    </tr>
    <tr>
      <td>5</td>
      <td><code>Maintain undelegate</code></td>
      <td>The amount of Energy to keep undelegated in the account. This amount is not used for orders.</td>
    </tr>
    <tr>
      <td>6</td>
      <td><code>Min price</code></td>
      <td>The minimum Energy price per day, in SUN/day, for orders you are willing to freeze for.</td>
    </tr>
    <tr>
      <td>7</td>
      <td><code>Min delegate amount</code></td>
      <td>The minimum resource amount that can be used to fill an order. This helps maximize the total resources used from your address.</td>
    </tr>
    <tr>
      <td>8</td>
      <td><code>Automatic Reclaim</code></td>
      <td><strong>TronSave:</strong> only reclaim resources delegated to others through the TronSave system. <strong>All:</strong> reclaim all resources delegated to others once the delegation is unlocked.</td>
    </tr>
  </tbody>
</table>

<figure><img src="https://1055070949-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fh0rlDWTk05Qu3lvB5Gve%2Fuploads%2FkitgkPcW9Uq7tfxHss1M%2FFrame%20137.png?alt=media&#x26;token=7735a4ae-f7fe-4e03-ad0a-aa2a72dbcccb" alt=""><figcaption></figcaption></figure>

<table>
  <thead>
    <tr><th>#</th><th>Setting</th><th>Description</th></tr>
  </thead>
  <tbody>
    <tr>
      <td>9</td>
      <td><code>Automatic Vote</code></td>
      <td>Automatically vote for Super Representatives to earn voting rewards.</td>
    </tr>
    <tr>
      <td>10</td>
      <td><code>Automatic Withdraw Reward</code></td>
      <td>Automatically claim and withdraw your voting rewards.</td>
    </tr>
    <tr>
      <td>11</td>
      <td><code>Automatic Stake</code></td>
      <td>Automatically stake when your balance reaches the threshold you set. The system runs a Staking 2.0 transaction to obtain Energy for you.</td>
    </tr>
  </tbody>
</table>

<figure><img src="https://1055070949-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fh0rlDWTk05Qu3lvB5Gve%2Fuploads%2FUthcklJHpefojpAGwtEC%2Fimage.png?alt=media&#x26;token=cd65d5e0-a268-4498-8921-b586fe104dff" alt=""><figcaption></figcaption></figure>

<table>
  <thead>
    <tr><th>#</th><th>Setting</th><th>Description</th></tr>
  </thead>
  <tbody>
    <tr>
      <td>12</td>
      <td><code>Payment Address</code></td>
      <td>The address that receives profit from Auto Sell.</td>
    </tr>
    <tr>
      <td>13</td>
      <td><code>Payment frequency</code></td>
      <td>How often payouts are made. <strong>Immediate</strong> after each filled order (fee 0.3 TRX per transaction), or <strong>Daily</strong> (zero fee).</td>
    </tr>
  </tbody>
</table>

## Next steps

* Learn the mechanics: [Staking 2.0](../../concepts/staking-2.0.md) · [Order Types](../../concepts/order-types.md) · [Pricing & APY](../../concepts/pricing-and-apy.md)
* Get resources to sell: [Get Energy by Staking 2.0](staking-2.0.md)
