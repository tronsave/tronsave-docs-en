---
description: Check whether TRON addresses are activated and activate up to 100 new addresses per request with your TronSave API key — 1.5 TRX per address.
---

# Activate Addresses

A new TRON address does not exist on-chain until it is **activated**: until then it can't receive a resource delegation, and the first transfer to it costs extra. Exchanges, payment processors and payroll tools that generate many deposit addresses usually need to activate them in bulk.

TronSave's activation API does two things:

| Endpoint | What it does | Auth | Cost |
| --- | --- | --- | --- |
| [Check Addresses](check-addresses.md) | Reports, for up to 100 addresses, whether each is activated, a contract, or invalid | None | Free |
| [Activate Addresses](activate-addresses.md) | Activates up to 100 addresses in one request | `apikey` | **1.5 TRX per address that needs activating**, from your internal account |

Addresses that are already active are skipped and not charged. Activation runs asynchronously after the request is accepted; call **Check Addresses** again to confirm.

## Before you start

* Get an API key and fund your internal account — see [Authentication](../../authentication.md).
* Keep enough TRX in the internal account for `1.5 × (number of inactive addresses)`.

{% hint style="info" %}
Need energy delivered to many addresses as well? See [Fast Charge](../fast-charge/README.md), or the **Bulk Buy** tool on the website. The **Bulk Send** tool can also activate inactive recipients while it sends tokens.
{% endhint %}
