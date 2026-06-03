---
description: Use send() to broadcast a contract call and optionally relay it through TronSave when your Energy is insufficient.
---

# send()

`send()` broadcasts a contract method call, just like the standard TronWeb `method.send()`. The TronSave SDK adds relay options so the transaction can be executed through TronSave's Energy when your own Energy is not enough.

You can pass any parameter you would normally pass to `method.send()`, plus the relay-specific options below.

* `useRelay: true` — call the method through **TronSave** if your Energy is insufficient to execute.
* `forceUseRelay: true` — always call the method through **TronSave**, even when you have enough Energy to execute. This has higher priority than `useRelay`.

{% tabs %}
{% tab title="Example" %}
```javascript
const onSend = async () => {
   const instance = await tronSave.contract().at("...")
   const testSend = await instance.method().send({
        useRelay: true,
        useSignType: "personal_trx",
        feeLimit: 1e9,
        ...
    })
}
```
{% endtab %}
{% endtabs %}

Reference: [https://developers.tron.network/reference/methodsend](https://developers.tron.network/reference/methodsend)

## Parameters

<table><thead><tr><th width="167">Params</th><th width="118">Type</th><th width="171">Default</th><th>Description</th></tr></thead><tbody><tr><td><code>useRelay</code></td><td>boolean</td><td>false</td><td>Call the method through <strong>TronSave</strong> if your Energy is insufficient to execute.</td></tr><tr><td><code>forceUseRelay</code></td><td>boolean</td><td>false</td><td>Always call the method through <strong>TronSave</strong>, even when you have enough Energy to execute. Priority is higher than <code>useRelay</code>.</td></tr><tr><td><code>useSignType</code></td><td>string</td><td>"personal_trx"</td><td><ul><li><code>personal_trx</code>: personal sign using <em>tronweb.trx.sign</em></li><li><code>typed_data</code>: use <em>tronweb.trx._signTypedData</em> (Currently, <strong>TronLink</strong> does not support this function)</li></ul></td></tr></tbody></table>

{% hint style="info" %}
`useSignType: "typed_data"` is not supported by TronLink. Use `personal_trx` (the default) when signing with TronLink.
{% endhint %}

## Next steps

* Learn about [Energy and Bandwidth](../../concepts/energy-and-bandwidth.md) to understand when a relay is needed.
* See the [Glossary](../../concepts/glossary.md) for terminology.
