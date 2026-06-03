---
description: Approve WTRX, check your allowance, and swap between TRX and WTRX using the TronSave SDK.
---

# WTRX Operations

[WTRX](../../concepts/glossary.md) (Wrapped TRX) is used in some SDK payment flows. Before the TronSave HUB contract can spend WTRX on your behalf, you must **approve** it. This page covers the three WTRX helpers exposed by the SDK: approving WTRX, reading your current allowance, and swapping TRX into WTRX.

{% hint style="info" %}
All examples assume you already have an initialized `tronSave` SDK instance. See [Connect](connect.md) for setup.
{% endhint %}

## Approve WTRX

You must approve WTRX with the HUB contract before sending a request that spends WTRX.

Use `approveTronSave()` with three parameters: `value` (the WTRX amount to approve), `options` (the [contract send options](https://developers.tron.network/reference/methodsend)), and `paymentMethod`.

{% tabs %}
{% tab title="Example" %}
```javascript
const onApproveWtrx = async () => {
    await tronSave.approveTronSave({
        value: value,
        options: options,
        paymentMethod: paymentMethod
    })
}
```
{% endtab %}
{% endtabs %}

<table><thead><tr><th>Params</th><th width="114">Default</th><th width="287">Description</th><th>Required</th></tr></thead><tbody><tr><td><code>value</code></td><td></td><td>WTRX amount to approve</td><td>true</td></tr><tr><td><code>options</code></td><td></td><td>Contract send options</td><td>false</td></tr><tr><td><code>paymentMethod</code></td><td>1</td><td>Approve TronSave for the given payment token</td><td>false</td></tr></tbody></table>

## Check WTRX Allowance

Check the approved WTRX allowance before sending a request. `getAllowanceTronSave(paymentMethod)` returns the current allowance value.

{% tabs %}
{% tab title="Example" %}
```javascript
const getAllowanceUser = async () => {
    await tronSave.getAllowanceTronSave(paymentMethod)
}
```
{% endtab %}
{% endtabs %}

<table><thead><tr><th width="185">Params</th><th width="163">Type</th><th width="162">Default</th><th width="136">Description</th><th>Required</th></tr></thead><tbody><tr><td><code>paymentMethod</code></td><td><code>1 | 2 | 10 | 11 | 12</code></td><td>1</td><td>See the <a href="payment-methods.md">payment method reference</a></td><td>false</td></tr></tbody></table>

## Swap TRX to WTRX

Quickly convert TRX into WTRX. `trxToWtrx()` takes two parameters: `value` (the TRX amount to deposit) and `options` (the [contract send options](https://developers.tron.network/reference/methodsend)).

{% tabs %}
{% tab title="Example" %}
```javascript
const depositTrx = async () => {
    const res = await tronSave.trxToWtrx({ value: value, options: options });
}
```
{% endtab %}
{% endtabs %}

<table><thead><tr><th>Params</th><th width="114">Default</th><th width="287">Description</th><th>Required</th></tr></thead><tbody><tr><td><code>value</code></td><td></td><td>TRX amount to deposit</td><td>true</td></tr><tr><td><code>options</code></td><td></td><td>Contract send options</td><td>false</td></tr></tbody></table>

## Typical flow

1. Swap the TRX you need into WTRX with `trxToWtrx()`.
2. Call `approveTronSave()` so the HUB contract can spend that WTRX.
3. Confirm the approval took effect with `getAllowanceTronSave()`.

## Next steps

- Review [WTRX in the Glossary](../../concepts/glossary.md).
- Continue with the [Developer Quickstart](../quickstart.md) to place your first order.
