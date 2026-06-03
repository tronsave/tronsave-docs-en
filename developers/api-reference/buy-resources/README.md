---
description: Two ways to buy Energy and Bandwidth through the TronSave API — Signed Transaction or API Key — and how to choose between them.
---

# Buy Resources

TronSave's API exposes two methods for purchasing resources (Energy and Bandwidth). Both let you estimate an order before placing it and then create the order; they differ in how the payment is authorized and settled.

| Method | Payment authorized by | Settles on-chain | Best for |
| --- | --- | --- | --- |
| [Signed Transaction](signed-tx/README.md) | Your private key signs each transaction | Yes, per order | Maximum control over funds; non-custodial flows |
| [API Key](api-key/README.md) | A TronSave API key on a prefunded internal account | No per-order chain fee | High-throughput, low-latency integrations |

## Using Signed Transaction

You build a transaction and sign it with your own private key. The signed transaction is submitted to the blockchain network for validation and execution, so only the holder of the private key can authorize the purchase.

**Advantages**

* High security — every order is signed with your private key.
* Direct control over payment funds; you manage them yourself.

**Disadvantages**

* Transactions submitted to the blockchain network incur additional transaction fees.
* More complex to integrate — you need to handle transaction signing and validation.

Endpoints:

* [Estimate Order](signed-tx/estimate-trx.md)
* [Create Order](signed-tx/create-order.md)

→ [Read the Signed Transaction guide](signed-tx/README.md)

## Using API Key

To use the API key method, create a TronSave internal account and deposit funds into it. Once the internal account exists, an API key is generated and used to authenticate your transactions — no per-order on-chain signing is required.

**Advantages**

* Fast and secure transactions.
* Easy integration.
* Saves transaction fees.

**Disadvantages**

* Funds must be deposited into the internal account before transactions can execute.
* TronSave manages the customer's internal account.

{% hint style="info" %}
Need an API key first? See [Get API Key](../../authentication.md).
{% endhint %}

Endpoints:

* [Estimate Order](api-key/estimate-trx.md)
* [Create Order](api-key/create-order.md)

→ [Read the API Key guide](api-key/README.md)

## Next steps

* Learn the difference between [Energy and Bandwidth](../../../concepts/energy-and-bandwidth.md).
* Review the [Order Types](../../../concepts/order-types.md) before placing orders.
