---
description: >-
  Official TronSave SDKs for buying and managing Energy and Bandwidth on TRON —
  available for TypeScript, Rust, Python, Java, and PHP.
---

# TRON Energy SDK — TypeScript, Rust, Python, Java, PHP

The **TronSave SDKs** let you interact with the TronSave API to manage resources (Energy and Bandwidth) on the TRON blockchain. Each SDK exposes strongly-typed functions for estimating costs, buying resources, extending orders, and tracking order status, and integrates cleanly into backend services.

{% hint style="info" %}
Prefer to call the HTTP API directly? See the [API Reference](api-reference/). The SDKs wrap the same endpoints documented there.
{% endhint %}

## Available SDKs

All SDKs target the **v2 API**.

| Language        | Package           | Registry                                                               | Latest |
| --------------- | ----------------- | ---------------------------------------------------------------------- | ------ |
| TypeScript / JS | `tronsave-sdk`    | [npm](https://www.npmjs.com/package/tronsave-sdk)                      | —      |
| Rust            | `tronsave`        | [crates.io](https://crates.io/crates/tronsave)                         | 2.0.0  |
| Python          | `tronsave`        | [PyPI](https://pypi.org/project/tronsave/)                             | 2.0.0  |
| Java            | `io.tronsave:sdk` | [Maven Central](https://central.sonatype.com/artifact/io.tronsave/sdk) | 2.0.0  |
| PHP             | `tronsave/sdk`    | [Packagist](https://packagist.org/packages/tronsave/sdk)               | 2.0.0  |

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

## Basic usage

The fastest way to get started is the API Key flow: create an SDK instance, estimate the cost, then place an order. Get an API key from the [TronSave dashboard](https://tronsave.io/market) — see [Authentication](authentication.md).

{% tabs %}
{% tab title="Node.js" %}
```javascript
import { TronsaveSDK } from "tronsave-sdk";

const main = async () => {
    const apiKey = "your_api_key";
    const sdk = new TronsaveSDK({ network: "testnet", apiKey });

    const userInfo = await sdk.getUserInfo();
    console.log(userInfo);

    // Example: estimate cost for 32,000 ENERGY for 1 hour
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
        durationSec: 3600,
        resourceAmount: 32000,
    });
    console.log(`Buy resource success -> orderId: ${orderId}`);

    // Wait ~5s for the order to be filled
    await new Promise((resolve) => setTimeout(resolve, 5000));

    const order = await sdk.getOrder(orderId);
    console.log(order.fulfilledPercent < 100 ? "Order is not filled" : "Order is filled");
};

main();
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
Set **`network: "mainnet"`** for production. The example uses **`"testnet"`** (Nile), where everything works the same way but uses no real TRX. See [Environments & Networks](environments.md).
{% endhint %}

## Features

All SDKs provide the following operations:

* **Buy Resource** — buy Energy or Bandwidth.
* **Estimate Buy Resource** — estimate the cost of a purchase before ordering.
* **Extend Request** — extend an existing resource delegation.
* **Get Extendable Delegates** — list delegates that can be extended.
* **Get User Info** — fetch account information and balance.
* **Get Order / Get Orders** — fetch order details and order history.
* **Get Order Book** — read the current order book.
