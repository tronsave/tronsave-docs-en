---
description: Official TronSave SDKs for buying and managing Energy and Bandwidth on TRON — available for TypeScript, Rust, Python, Java, and PHP.
---

# SDK

The **TronSave SDKs** let you interact with the TronSave API to manage resources (Energy and Bandwidth) on the TRON blockchain. Each SDK exposes strongly-typed functions for estimating costs, buying resources, extending orders, and tracking order status, and integrates cleanly into backend services and browser dApps.

{% hint style="info" %}
Prefer to call the HTTP API directly? See the [API Reference](../api-reference/README.md). The SDKs wrap the same endpoints documented there.
{% endhint %}

## Available SDKs

All SDKs target the **v2 API**.

| Language | Package | Registry | Latest |
| --- | --- | --- | --- |
| TypeScript / JS | `tronsave-sdk` | [npm](https://www.npmjs.com/package/tronsave-sdk) | — |
| Rust | `tronsave` | [crates.io](https://crates.io/crates/tronsave) | 2.0.0 |
| Python | `tronsave` | [PyPI](https://pypi.org/project/tronsave/) | 2.0.0 |
| Java | `io.tronsave:sdk` | [Maven Central](https://central.sonatype.com/artifact/io.tronsave/sdk) | 2.0.0 |
| PHP | `tronsave/sdk` | [Packagist](https://packagist.org/packages/tronsave/sdk) | 2.0.0 |

## Install

{% tabs %}
{% tab title="npm" %}
```bash
npm install tronsave-sdk
# or: yarn add tronsave-sdk
```

Requires **Node.js v18.0.0+**.
{% endtab %}

{% tab title="Rust" %}
```bash
cargo add tronsave
```

Or in `Cargo.toml`:

```toml
[dependencies]
tronsave = "2.0.0"
```
{% endtab %}

{% tab title="Python" %}
```bash
pip install tronsave
```

Requires **Python 3.9+**.
{% endtab %}

{% tab title="Java" %}
Maven (`pom.xml`):

```xml
<dependency>
    <groupId>io.tronsave</groupId>
    <artifactId>sdk</artifactId>
    <version>2.0.0</version>
</dependency>
```

Gradle (`build.gradle`):

```groovy
implementation 'io.tronsave:sdk:2.0.0'
```
{% endtab %}

{% tab title="PHP" %}
```bash
composer require tronsave/sdk
```

Requires **PHP 8.1+**.
{% endtab %}
{% endtabs %}

## Connect and buy

The fastest way to get started is the API Key flow: create an SDK instance, estimate the cost, then place an order. Get an API key from the [TronSave dashboard](https://tronsave.io/market) — see [Authentication](../authentication.md) for details.

{% tabs %}
{% tab title="Node.js" %}
```javascript
import { TronsaveSDK } from "tronsave-sdk";

const main = async () => {
    const apiKey = "your_api_key";
    const sdk = new TronsaveSDK({ network: "testnet", apiKey });

    const userInfo = await sdk.getUserInfo();
    console.log(userInfo);

    // Example: Estimate cost for 32,000 ENERGY units for 1 hour
    const estimate = await sdk.estimateBuyResource({
        receiver: "TAk6jzZqHwNUkUcbvMyAE1YAoUPk7r2T6h",
        resourceType: "ENERGY",
        durationSec: 3600, // 1 hour
        resourceAmount: 32000,
    });
    console.log(estimate);
    if (estimate.estimateTrx > Number(userInfo.balance)) throw new Error("Insufficient balance");

    const { orderId } = await sdk.buyResource({
        receiver: "TAk6jzZqHwNUkUcbvMyAE1YAoUPk7r2T6h",
        resourceType: "ENERGY",
        durationSec: 3600, // 1 hour
        resourceAmount: 32000,
    });
    console.log(`Buy resource success -> orderId: ${orderId}`);

    // Wait for 5 seconds for the order to be filled
    console.log("Waiting for 5 seconds for the order to be filled...");
    await new Promise((resolve) => setTimeout(resolve, 5000));

    const order = await sdk.getOrder(orderId);
    console.log(order);
    if (order.fulfilledPercent < 100) console.log("Order is not filled");
    else console.log("Order is filled");
};

main();
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
Set `network: "mainnet"` for production. The example above uses `"testnet"` (Nile), where everything works the same way but uses no real TRX. See [Environments & Networks](../environments.md).
{% endhint %}

## Features

The SDK provides the following operations:

* **Buy Resource** — buy Energy or Bandwidth.
* **Estimate Buy Resource** — estimate the cost of a resource purchase before ordering.
* **Extend Request** — extend an existing resource delegation.
* **Get Extendable Delegates** — list delegates that can be extended.
* **Get User Info** — fetch account information and balance.
* **Get Order / Get Orders** — fetch order details and order history.
* **Get Order Book** — read the current order book.

## Browser dApp flow

In a browser dApp you can connect through an injected `tronWeb` instance instead of an API key, estimate the Energy a transaction will consume, and pay from the connected wallet. This flow is covered across the child pages below — start with [Connect](connect.md).

## Next steps

* [Connect](connect.md) — initialize the SDK with an API key or injected `tronWeb`.
* [Send](send.md) — send transactions and buy Energy.
* [Estimate Energy](estimate-energy.md) — estimate Energy required for a transaction.
* [Payment Methods](payment-methods.md) — choose how purchases are paid for.
* [WTRX (Approve / Allowance / Swap)](wtrx.md) — work with wrapped TRX.
* [Contract Addresses](contract-addresses.md) — on-chain contract references.
* [Config & History](config-and-history.md) — configuration options and order history.
