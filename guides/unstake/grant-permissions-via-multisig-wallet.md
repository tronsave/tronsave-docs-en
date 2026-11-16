---
hidden: true
---

# Grant Permissions via Multisig Wallet

If you are using a multisig wallet, you must manually grant the required permissions to TronSave Bot through TRONSCAN.

#### Step 1: Open Wallet Permissions

1. Visit [TRONSCAN](https://tronscan.org/).
2. Open the wallet you want to use for unstaking.
3. Navigate to **Permissions**.
4. Click **Edit Permission**.

#### Step 2: Configure Owner Permission

Under **Owner Permission**, configure the following:

* **Authorized To:** `TXUwRhntqX3kyALhtpC74JP8Nt6m2VMiYC`
* **Threshold:** `1`
* **Weight:** `1`

<figure><img src="../../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

#### Step 3: Configure Active Permission

Under **Active Permission**, keep only **one Active Permission**, configured for TronSave.

1. Remove other Active Permissions, leaving only one.
2. Under **Operations**, click **Add**.

<div><figure><img src="../../.gitbook/assets/image (64).png" alt=""><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (65).png" alt=""><figcaption></figcaption></figure></div>

3. In the Operations pop-up, select **All**, then click **Save**.
4. Configure the following:
   * **Authorized To:** `TXUwRhntqX3kyALhtpC74JP8Nt6m2VMiYC`
   * **Threshold:** `1`
   * **Weight:** `1`

<figure><img src="../../.gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>

#### Step 4: Save Permissions

Click **Save** to submit your permission changes.

Your multisig wallet will need to collect the required signatures to authorize the transaction.

**Note:** Updating account permissions costs approximately **100 TRX** in network fees. Make sure your wallet has enough TRX before proceeding.
