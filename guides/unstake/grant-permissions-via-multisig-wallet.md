---
hidden: true
---

# Grant Permissions via Multisig Wallet

If your wallet uses a multisig setup, follow these steps to update Early Unstake permissions.

{% tabs %}
{% tab title="PC (TRONSCAN)" %}
#### Step 1: Open the Authorized Wallet on TRONSCAN

1. Go to [TRONSCAN](https://tronscan.org/wallet/permissions).
2. Connect **Wallet B** – the wallet that was granted Owner Permission by your unstaking wallet (Wallet A).
3. Navigate to **Other Permission** under **Owner Permission**.
4. Find and select **Wallet A** – the wallet you want to unstake from.
5. Click the **Edit icon.**

<figure><img src="../../.gitbook/assets/image (71).png" alt=""><figcaption></figcaption></figure>

#### Step 2: Update Owner Permission

Under **Owner Permission**, configure:

* **Authorized To:** `TXUwRhntqX3kyALhtpC74JP8Nt6m2VMiYC`
* **Threshold:** `1`
* **Weight:** `1`

#### Step 3: Update Active Permission

Under **Active Permission**, keep only **one Active Permission** for TronSave.

1. Remove other Active Permissions.
2. Under **Operations**, click **Add**.

<div><figure><img src="../../.gitbook/assets/image (74).png" alt=""><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (65).png" alt=""><figcaption></figcaption></figure></div>

3. In the Operations pop-up, select **All**, then click **Save**.
4. Configure the following:
   * **Authorized To:** `TXUwRhntqX3kyALhtpC74JP8Nt6m2VMiYC`
   * **Threshold:** `1`
   * **Weight:** `1`

<figure><img src="../../.gitbook/assets/image (75).png" alt=""><figcaption></figcaption></figure>

#### Step 4: Save the Changes

* Click **Save** to update the permissions. The transaction requires approximately **100 TRX** in network fees. Make sure **Wallet A** has at least **100 TRX** available.

**Note:**

* _If **Wallet A** requires signatures from multiple multisig wallets (e.g., **Wallet B** and **Wallet C**), connect **Wallet B** first to update the permissions. Then, connect **Wallet C** to TRONSCAN to provide the remaining required signature._

<figure><img src="../../.gitbook/assets/image (76).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="TronLink Extension" %}
#### Step 1: Open the Authorized Wallet on TronLink Extension

1. Log in to the **TronLink Extension** with **Wallet B** – the wallet that was granted Owner Permission by your unstaking wallet (Wallet A).
2. Choose "**More**" > "**Account Permissions**" to enter the corresponding webpage.

<div><figure><img src="../../.gitbook/assets/image (77).png" alt=""><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (78).png" alt=""><figcaption></figcaption></figure></div>

4. Navigate to **Other Permission** under **Owner Permission**.
5. Find and select **Wallet A** – the wallet you want to unstake from.
6. Click the **Edit icon.**

<figure><img src="../../.gitbook/assets/image (79).png" alt=""><figcaption></figcaption></figure>

#### Step 2: Update Owner Permission

Under **Owner Permission**, configure:

* **Authorized To:** `TXUwRhntqX3kyALhtpC74JP8Nt6m2VMiYC`
* **Threshold:** `1`
* **Weight:** `1`

#### Step 3: Update Active Permission

Under **Active Permission**, keep only **one Active Permission** for TronSave.

1. Remove other Active Permissions.
2. Under **Operations**, click **Add**.
3. In the Operations pop-up, select **All**, then click **Save**.
4. Configure the following:
   * **Authorized To:** `TXUwRhntqX3kyALhtpC74JP8Nt6m2VMiYC`
   * **Threshold:** `1`
   * **Weight:** `1`

<figure><img src="../../.gitbook/assets/image (80).png" alt=""><figcaption></figcaption></figure>

#### Step 4: Save the Changes

* Click **Save** to update the permissions. The transaction requires approximately **100 TRX** in network fees. Make sure **Wallet A** has at least **100 TRX** available.

**Note:**

* _If **Wallet A** requires signatures from multiple multisig wallets (e.g., **Wallet B** and **Wallet C**), connect **Wallet B** first to update the permissions. Then, connect **Wallet C** to TRONSCAN to provide the remaining required signature._

<figure><img src="../../.gitbook/assets/image (81).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

