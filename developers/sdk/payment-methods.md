---
description: Configure who pays transaction fees in your dApp — Gas Enterprise (developer-funded) and Gas-paid WTRX (user-funded) — and the payment-method codes the SDK accepts.
---

# Payment Methods

When you integrate TronSave into a dApp, you decide who pays the on-chain transaction fee and in which token. TronSave exposes this choice as a set of **payment-method codes** you pass to the SDK, plus two delivery models: **Gas Enterprise** (the developer sponsors fees) and **Gas-paid WTRX** (the end user pays fees in a token they already hold).

## Payment-method codes

Pass the payment method to the SDK via the `paymentMethodType` field. The SDK accepts the following values:

<table>
  <thead>
    <tr><th>Code</th><th>Description</th></tr>
  </thead>
  <tbody>
    <tr><td><code>1</code></td><td>On-chain WTRX — users pay transaction fees with WTRX (default).</td></tr>
    <tr><td><code>11</code></td><td>The dApp pays transaction fees on behalf of the user.</td></tr>
    <tr><td><code>12</code></td><td>The dApp pays transaction fees, but takes a token from the user in return.</td></tr>
    <tr><td><code>2</code>, <code>10</code>, <code>4</code></td><td>Coming soon.</td></tr>
  </tbody>
</table>

{% hint style="info" %}
Codes `2`, `10`, and `4` are not yet available.
{% endhint %}

## Gas Enterprise (developer-funded)

With Gas Enterprise, your project maintains a gas tank and pays transaction fees on behalf of your users. This corresponds to payment method `11` (developers pay fees in TRX). Your users transact without needing TRX in their own wallets.

To set this up, register your dApp on the TronSave dashboard and fund your project:

* **Mainnet dashboard:** [dashboard.tronsave.io](https://dashboard.tronsave.io/)
* **Testnet dashboard:** [testnet.dashboard.tronsave.io](https://testnet.dashboard.tronsave.io)

Steps:

1. **Deposit and create your first project.**

{% embed url="https://youtu.be/u1tygbEFTYU" %}

2. **Add a contract and set up the contract method.**

{% embed url="https://youtu.be/tAU1zwrl89A" %}

There is no published minimum deposit or gas-tank top-up threshold for Gas Enterprise — fund your project with whatever balance suits your expected transaction volume.

## Gas-paid WTRX (user-funded)

Today, users without native tokens in their wallet have to exit the dApp, purchase or swap for native tokens, and then come back to send a transaction. Enabling gas payments in WTRX lets your users pay gas fees in a token they already hold. TronSave currently supports gas-fee payments using TRX and WTRX, with plans to support more tokens soon. This corresponds to payment method `1` (users pay fees with WTRX).

This feature improves UX for your end users in two main ways:

1. Allowing your end users to save their TRX, or the native token, on the blockchain they're interacting on.
2. No stuck transactions when using dApps.

### Flow explained

This feature is powered by TronSave's upgraded relayer infrastructure.

In this flow:

1. The relayer shares a list of supported tokens, which you display to your users in your UI.
2. Your user chooses which supported token to pay gas fees in.
3. You show the estimated gas to be paid using that WTRX token.
4. Once the user confirms, the transaction is sent to the relayer.
5. The relayer pays for gas in the native token and sends the transaction to the chain.
6. The relayer is refunded from the user's smart-contract wallet in WTRX.

<figure><img src="https://content.gitbook.com/content/tWFcrYCIaFhC14CKjggz/blobs/FXKKVxbhxOF0SoUtQqJ4/Screen%20Shot%202023-02-01%20at%2016.53.44.png" alt=""><figcaption></figcaption></figure>

## Next steps

* [Developer Quickstart](../quickstart.md) — make your first TronSave API call.
* [Authentication](../authentication.md) — compare API Key and Signed Transaction methods.
* [Glossary](../../concepts/glossary.md) — definitions for Energy, Bandwidth, TRX, SUN, and WTRX.
