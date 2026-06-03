---
description: Grant TronSave the permissions it needs to delegate resources and earn as a Provider — auto setup in-app, or manual setup in TronScan/TronLink.
---

# Permission

As a **Provider**, you stake TRX, receive [Energy or Bandwidth](../../concepts/energy-and-bandwidth.md), and let TronSave delegate that resource to buyers so you earn up to **25% APY**. Becoming a Provider requires staking at least **5,000 TRX**.

For TronSave to manage delegation, staking, voting, and reward withdrawal on your behalf, you must grant it an **active permission** on your account using TRON multi-signature. You can do this automatically inside TronSave, or manually in TronScan / TronLink.

{% hint style="info" %}
TronSave only ever receives an **active permission** (not owner permission). It can act on the specific operations you authorize below — it cannot transfer ownership of your account.
{% endhint %}

## 1. Auto permission (in TronSave)

The fastest path. TronSave builds the permission transaction for you.

1. Open the **Seller** tab at [tronsave.io/dashboard/seller](https://tronsave.io/dashboard/seller) and click **Login TRONSAVE**.
2. Choose **Settings**, then click **Register**.

<figure><img src="https://1055070949-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fh0rlDWTk05Qu3lvB5Gve%2Fuploads%2F0HqY5O2AYZXdvgMsewM4%2Fanh1.png?alt=media&#x26;token=9241d620-85b3-4d97-bc72-bbdf0798ae7e" alt=""><figcaption></figcaption></figure>

3. Select the permissions for the pool according to your preferences, then click **Give Permission**.

<figure><img src="https://1055070949-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fh0rlDWTk05Qu3lvB5Gve%2Fuploads%2FhAUmsURVlfnWuvHcIzlj%2Fimage.png?alt=media&#x26;token=f12d4a04-31c4-431e-aad9-b697e0921873" alt=""><figcaption></figcaption></figure>

## 2. Manual permission (in TronScan / TronLink)

If you prefer to configure the active permission yourself, add the following operations and assign them to the TronSave address.

### Operations to grant

* _Resources Delegate_
* _Resource Reclaim_
* _TRX Stake 2.0_
* _TRX Unstake 2.0_
* _Vote_
* _Reward Withdraw_

### TronSave address

Add this address in the **Keys** box of the new active permission:

```
TXUwRhntqX3kyALhtpC74JP8Nt6m2VMiYC
```

{% tabs %}
{% tab title="PC (TronScan)" %}
1. Open [https://tronscan.org/#/wallet/account](https://tronscan.org/#/wallet/account).
2. Connect your wallet.
3. Click **Edit Permission** in the **Owner Access** section.
4. In the **Active Permission** section, click **+ Add active permission**.
5. In the **Add Active Permission** pop‑up, under **Permission Name**, enter anything (e.g. `Tronsave`). Click the **+ Add** action button and select the six [operations listed above](#operations-to-grant).
6. Click **Save**. In the **Threshold** box, enter `1`. In the **Weight** box, enter `1`, and put the TronSave address `TXUwRhntqX3kyALhtpC74JP8Nt6m2VMiYC` in the **Keys** box.
7. Click **Save**. The **Add active permission** pop‑up closes.
8. Click **Save** in the **Owner Permission** section to complete the multi‑sig process.

<figure><img src="https://1055070949-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fh0rlDWTk05Qu3lvB5Gve%2Fuploads%2FU6idiv1eNz5b6VG9bwKT%2Fimage.png?alt=media&#x26;token=f9909553-7118-455c-b870-0e1cba68cc7c" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Mobile (TronLink)" %}
1. Log in to your TronLink app.
2. Tap **Me** in the bottom‑right corner.
3. Tap **Public Account Management**, then **Permission**.
4. Tap **Add Permissions**.
5. Under **Permission Name**, enter anything (e.g. `Tronsave`).
6. In the **Operations** section, tap the pen symbol and select the six [operations listed above](#operations-to-grant).
7. Tap **Confirm**. In the **Threshold** box, enter `1`. In the **Weight** box, enter `1`, and put the TronSave address `TXUwRhntqX3kyALhtpC74JP8Nt6m2VMiYC` in the **Keys** box.
8. Tap **Confirm**.

![](https://1055070949-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fh0rlDWTk05Qu3lvB5Gve%2Fuploads%2Frz2PfD0ZSZLVbwgayim9%2Fimage.png?alt=media\&token=2be78044-977e-43ea-b0de-e50d6185025b)![](https://1055070949-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fh0rlDWTk05Qu3lvB5Gve%2Fuploads%2FVjUGfKPxii1u97nj2wbA%2Fimage.png?alt=media\&token=29888fc9-878b-4eca-9d76-1b8838c16f85)
{% endtab %}
{% endtabs %}

## Next steps

* [Sell / Provider guide](README.md) · [Staking 2.0](../../concepts/staking-2.0.md) · [Pricing & APY](../../concepts/pricing-and-apy.md)
