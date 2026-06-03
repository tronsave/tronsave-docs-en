---
description: Version history and breaking changes for the TronSave API.
---

# Changelog

This page tracks notable changes to the TronSave API across its generations. Two API generations currently exist.

## API generations

| Generation | Base path | Status | Notes |
| --- | --- | --- | --- |
| **v2** | `https://api.tronsave.io/v2` | Current | The recommended generation for all new integrations. Endpoints in the [API Reference](api-reference/README.md) target v2. |
| **v0** | `https://api.tronsave.io/v0` | Legacy (still supported) | Earlier generation, still supported and maintained for backward compatibility. There is no deprecation or sunset date. New integrations should use v2. |

{% hint style="info" %}
On the Nile testnet, replace the host with `https://api-dev.tronsave.io` (e.g. `https://api-dev.tronsave.io/v2`). See [Quickstart → Testing first?](../getting-started/quickstart.md).
{% endhint %}

{% hint style="warning" %}
v0 is considered legacy but is still supported and maintained. There is no deprecation or sunset date. We recommend migrating to v2 for new integrations.
{% endhint %}

## Release history

<!-- [NEEDS CONFIRMATION: real changelog entries and dates — the entries below are placeholders and must be replaced with confirmed releases] -->

### Unreleased

* _Placeholder — no confirmed dated entries yet._

<!--
Template for future entries:

### YYYY-MM-DD — vX.Y

**Added**
* ...

**Changed**
* ...

**Fixed**
* ...

**Deprecated / Removed**
* ...
-->

## Next steps

* [Authentication](authentication.md) · [API Reference](api-reference/README.md)
* [Developer Quickstart](quickstart.md)
