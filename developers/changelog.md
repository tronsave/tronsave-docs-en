---
description: Version history and breaking changes for the TronSave API.
---

# Changelog

This page tracks notable changes to the TronSave API across its generations. Two API generations currently exist.

## API generations

<table><thead><tr><th width="123">Generation</th><th width="152">Base path</th><th width="129">Status</th><th>Notes</th></tr></thead><tbody><tr><td><strong>v2</strong></td><td><code>https://api.tronsave.io/v2</code></td><td>Current</td><td>The recommended generation for all new integrations. Endpoints in the <a href="api-reference/">API Reference</a> target v2.</td></tr><tr><td><strong>v0</strong></td><td><code>https://api.tronsave.io/v0</code></td><td>Legacy (still supported)</td><td>The earlier generation is still supported and maintained for backward compatibility. There is no deprecation or sunset date. New integrations should use v2.</td></tr></tbody></table>

{% hint style="info" %}
On the Nile testnet, replace the host with `https://api-dev.tronsave.io` (e.g. `https://api-dev.tronsave.io/v2`). See [Quickstart → Testing first?](../getting-started/quickstart.md).
{% endhint %}

{% hint style="warning" %}
v0 is considered legacy but is still supported and maintained. There is no deprecation or sunset date. We recommend migrating to v2 for new integrations.
{% endhint %}

## Release history

### Unreleased

* _Placeholder — no confirmed dated entries yet._

## Next steps

* [Authentication](authentication.md) · [API Reference](api-reference/)
* [Developer Quickstart](quickstart.md)
