---
description: Install the TronSave SDK, connect it to TronWeb, and check your WTRX payment balance.
---

# Connect the SDK

The `@tronsave/sdk` package lets you estimate, buy, and confirm Energy orders directly from your application using TronWeb. This page covers installation, connecting the SDK in browser and server environments, the available config options, and how to check your payment balance.

{% hint style="info" %}
Latest SDK version is **1.1.34**.
{% endhint %}

## Install

The SDK depends on `tronweb`, so install both:

{% tabs %}
{% tab title="npm" %}
```bash
npm i @tronsave/sdk tronweb
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @tronsave/sdk tronweb
```
{% endtab %}
{% endtabs %}

Before you connect, register your project and obtain an API key. See [Authentication](../authentication.md) for how to get a key and set up your Internal Account.

{% hint style="warning" %}
Without a valid API key you cannot use the `useRelay` or `forceUseRelay` options.
{% endhint %}

## Connect

`TronSave` is initialized with a TronWeb instance and an `options` object. The `options` configure how requests are validated and how payment transactions are built.

{% tabs %}
{% tab title="Browser" %}
TronSave can only be initialized once `tronWeb` is injected into the page (for example by a wallet extension), so wait for it before constructing the instance:

```typescript
import { TronSave } from '@tronsave/sdk'

// TronSave can only be initialized when tronWeb already exists
let tronSave = null;

const waitTronSave = () => {
  return new Promise((res) => {
    let attempts = 0,
      maxAttempts = 20;
    const checkTronWeb = () => {
      if ((window as any).tronWeb?.defaultAddress.base58) {
        const options = {
          apiKey: API_KEY,
          networks: "mainnet",
          // ...other options — see the table below
        };
        res(new TronSave(window.tronWeb, options));
        return;
      }
      attempts++;
      if (attempts >= maxAttempts) {
        res(null);
        return;
      }
      setTimeout(checkTronWeb, 100);
    };
    checkTronWeb();
  });
};

waitTronSave().then((res) => {
  if (res) {
    tronSave = res;
  }
});
```
{% endtab %}

{% tab title="Server" %}
On the server, construct your own TronWeb instance with a private key:

```javascript
import { TronSave } from '@tronsave/sdk'
import TronWeb from 'tronweb'

const tronWeb = new TronWeb({
  fullHost: 'https://api.trongrid.io',
  // ...
  privateKey: 'your private key'
})

const options = {
  apiKey: API_KEY,
  networks: "mainnet",
  // ...
}

const tronSave = new TronSave(tronWeb, options)
```
{% endtab %}
{% endtabs %}

## Config options

<table><thead><tr><th width="223">Params</th><th width="149">Type</th><th width="269">Description</th><th>Required</th></tr></thead><tbody><tr><td><code>apiKey</code></td><td>string</td><td>dApp API key. If you don't have a valid API key, you cannot use the <strong>useRelay</strong> or <strong>forceUseRelay</strong> option.</td><td>false</td></tr><tr><td><code>networks</code></td><td>"<strong>mainnet</strong>" | "<strong>nile</strong>"</td><td>Node network: <strong>mainnet</strong> or <strong>nile</strong></td><td>true</td></tr><tr><td><code>domainName</code></td><td>string</td><td>Domain name</td><td>false</td></tr><tr><td><code>name</code></td><td>string</td><td>Project name</td><td>false</td></tr><tr><td><code>version</code></td><td>string</td><td>Project version</td><td>false</td></tr><tr><td><code>chainId</code></td><td>string</td><td>Blockchain chain id</td><td>false</td></tr><tr><td><code>verifyingContract</code></td><td>string</td><td>Address</td><td>false</td></tr><tr><td><code>validAfter</code></td><td>number</td><td>The highest block number the request can be forwarded in, or 0 if request validity is not time-limited</td><td>false</td></tr><tr><td><code>paymentMethodType</code></td><td>enum (default <strong>1</strong>)</td><td>1 - Onchain WTRX (default)<br>11 - dApp pay for user<br>12 - dApp pay for the user but take a token<br>2 / 10 / 4 - coming soon</td><td>false</td></tr></tbody></table>

{% hint style="info" %}
The config key is `networks` (not `network`), with value `"mainnet"` or `"nile"`.
{% endhint %}

## Check payment balance

Use `getBalancePayment(paymentMethod)` to read your WTRX balance for a given payment method.

{% tabs %}
{% tab title="Example" %}
```javascript
const getBalance = async () => {
  await tronSave.getBalancePayment(paymentMethod)
}
```
{% endtab %}
{% endtabs %}

<table><thead><tr><th width="190">Params</th><th width="164">Type</th><th width="117">Default</th><th width="220">Description</th><th>Required</th></tr></thead><tbody><tr><td><code>paymentMethod</code></td><td>1 | 2 | 10 | 11 | 12</td><td>1</td><td>See the <code>paymentMethodType</code> values in the config options table above.</td><td>false</td></tr></tbody></table>

## Next steps

Once connected, continue with the full purchase flow: estimate the Energy a transaction needs, estimate the TRX payout, buy Energy, and confirm the order status. See the [API Reference](../api-reference/README.md) and [Developer Quickstart](../quickstart.md) for the end-to-end flow.
