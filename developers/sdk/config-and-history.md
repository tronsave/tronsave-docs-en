---
description: Read the active TronSave SDK configuration and fetch an end user's transaction history.
---

# Config & History

Two read-only helpers on the TronSave SDK instance: inspect the configuration the SDK is currently running with, and pull the history of transactions placed through TronSave for a given address.

{% hint style="info" %}
Both methods are called on an initialized `tronSave` instance. See [Connect TronSave](./connect.md) for how to create that instance.
{% endhint %}

## Show current config

`showConfig()` returns the configuration the SDK is currently using.

{% tabs %}
{% tab title="Example" %}
```tsx
const showConfig = () => {
    const configs = tronSave.showConfig()
}
```
{% endtab %}
{% endtabs %}

The exact shape of the returned object is not formally documented. Inspect the value at runtime (e.g. `console.log(tronSave.showConfig())`) to see the fields available in your SDK version.

## Get end user history

`getHistory()` returns the history of all transactions invoked through TronSave for the given address. The result is a JSON string, so parse it before use.

{% tabs %}
{% tab title="Example" %}
```javascript
const handleGetHistory = async (address, from, to, page, pageSize) => {
    const dataHistory = await tronSave.getHistory(address, from, to, page, pageSize)
    console.log(JSON.parse(dataHistory))
}
```
{% endtab %}
{% endtabs %}

### Parameters

<table>
  <thead>
    <tr><th>Parameter</th><th>Description</th></tr>
  </thead>
  <tbody>
    <tr><td><code>address</code></td><td>The TRON address whose transaction history is requested.</td></tr>
    <tr><td><code>from</code></td><td>Start of the time range to query. <!-- [NEEDS CONFIRMATION: exact type/format of `from` — timestamp, ms, or ISO string] --></td></tr>
    <tr><td><code>to</code></td><td>End of the time range to query. <!-- [NEEDS CONFIRMATION: exact type/format of `to` — timestamp, ms, or ISO string] --></td></tr>
    <tr><td><code>page</code></td><td>Page number for pagination. <!-- [NEEDS CONFIRMATION: is `page` 0-indexed or 1-indexed] --></td></tr>
    <tr><td><code>pageSize</code></td><td>Number of records returned per page. <!-- [NEEDS CONFIRMATION: max allowed pageSize] --></td></tr>
  </tbody>
</table>

{% hint style="warning" %}
`getHistory()` resolves to a JSON **string**. Wrap the call in `JSON.parse()` (as shown above) before reading the data.
{% endhint %}

The shape of each history record is not formally documented. `getHistory()` returns a JSON **string**, so call `JSON.parse()` and inspect the resulting object at runtime to see the fields available in your SDK version.

## Next steps

* [Connect TronSave](./connect.md) — initialize the SDK instance used by these methods.
* [Glossary](../../concepts/glossary.md) — terminology used across the docs.
