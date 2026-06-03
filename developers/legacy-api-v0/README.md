---
description: The legacy TronSave REST API v0 — kept for existing integrations. New projects should use v2.
---

# Legacy API (v0)

REST API v0 was the original TronSave API for buying Energy programmatically. It supports two ways to pay for and authorize an order: a **signed transaction** or a **TronSave API key**.

{% hint style="warning" %}
**v0 is legacy.** It is maintained for backward compatibility with existing integrations only. New integrations should use the current [API Reference (v2)](../api-reference/README.md), which adds Bandwidth support, a redesigned structure, more endpoints, and flexible payment options.
{% endhint %}

## Why upgrade to v2

REST API v2 was released with significant improvements over v0:

* **New resource support: Bandwidth** — v0 buys Energy only.
* **Redesigned structure** for easier integration and a better developer experience.
* **More comprehensive features** to better support your use cases.
* **Flexible payment options** — both signed transactions and internal-account payments.

If you are currently on v0, upgrade to [v2](../api-reference/README.md) to take full advantage of these enhancements.

## Payment methods

There are two methods for purchasing Energy using a TronSave API key and a signed transaction.

<table>
  <thead>
    <tr><th width="220">Method</th><th>Summary</th></tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Signed transaction</strong></td>
      <td>You build a transaction and sign it with your private key. The signed transaction is sent to the network for validation and execution, so only the owner of the private key can perform it.</td>
    </tr>
    <tr>
      <td><strong>API key</strong></td>
      <td>You create a TronSave internal account, deposit funds, and authenticate each request with the generated API key.</td>
    </tr>
  </tbody>
</table>

### Using a signed transaction

* You create a transaction and sign it with your private key.
* The signed transaction is then sent to the blockchain network for validation and execution. This ensures that only the owner of the private key can perform the transaction.

**Advantages**

* A high level of security is required when signing with a private key.
* More control over payment funds, as customers manage them directly.

**Disadvantages**

* Transactions sent to the blockchain network will incur additional transaction fees.
* More complex integration — requires an understanding of transaction signing and validation processes.

See the legacy code example: [Buy Energy with a Private Key](../code-examples/v0/buy-with-private-key.md).

### Using an API key

* To use the API key, you need to create a TronSave internal account and deposit funds into it. See [Authentication](../authentication.md).
* After creating the internal account, an API key will be generated, which is used to authenticate your transactions.

**Advantages**

* Fast and secure transactions.
* Easy integration.
* Saves transaction fees.

**Disadvantages**

* Funds need to be deposited into the internal account to execute transactions.
* The customer's internal account is managed by the TronSave system.

See the legacy code examples: [Buy Energy with an API Key](../code-examples/v0/buy-with-api-key.md) and [Extend an Order with an API Key](../code-examples/v0/extend-with-api-key.md).

## Next steps

* [API Reference (v2)](../api-reference/README.md) — the current API for all new integrations.
* [Code Examples](../code-examples/README.md) — runnable v0 and v2 scripts.
* [Authentication](../authentication.md) — obtain an API key and fund your internal account.
