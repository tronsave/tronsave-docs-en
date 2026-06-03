---
description: >-
  Make your first TronSave API call — get a key, estimate, buy, and confirm an
  Energy order in about 10 minutes.
---

# Developer Quickstart

This guide walks you through your first TronSave API call — from getting an API key to having your first Energy order successfully filled — in about **10 minutes**.

## Before you begin

TronSave supports **two authentication methods** for API calls. Choose the one that fits your use case:

<table><thead><tr><th width="166"></th><th>API Key (Internal Account)</th><th>Signed Transaction</th></tr></thead><tbody><tr><td><strong>Best for</strong></td><td>Automated bots, backend services</td><td>When you prefer not to hold TRX in TronSave</td></tr><tr><td><strong>How it works</strong></td><td>Deposit TRX into your internal account; TronSave deducts costs automatically</td><td>Sign a TRX transaction from your own wallet on each purchase</td></tr><tr><td><strong>Complexity</strong></td><td>Simpler</td><td>Requires TronWeb integration</td></tr><tr><td><strong>Recommended</strong></td><td>For most use cases</td><td>When you need full custody</td></tr></tbody></table>

{% hint style="info" %}
**This guide uses the API Key method.** If you want to use Signed Transaction instead, see [Buy Resources → Signed Transaction](api-reference/buy-resources/signed-tx/). For a deeper comparison, see [Authentication](authentication.md).
{% endhint %}

## Step 1 — Get an API key and deposit TRX

### 1.1 Get your API key

1. Go to [tronsave.io/market](https://tronsave.io/market) and click **Connect** to connect your wallet.
2. After connecting, click the **Address** button → select **Account Info** → click **Login TronSave** and sign in to confirm.
3. Your API key and deposit address will be displayed — copy and save them.

{% hint style="info" %}
**Testing?** Use the Nile testnet at [testnet.tronsave.io](https://testnet.tronsave.io/) — everything works the same way but uses no real TRX. See the [Test Environment](quickstart.md#test-environment-nile-testnet) table below.
{% endhint %}

### 1.2 Deposit TRX into your internal account

Click **Top Up** to get your deposit address, then send TRX to that address from any wallet.

A few things to note:

* Minimum deposit is **10 TRX** per transaction.
* Your first deposit requires an extra \~1 TRX to activate the new address.
* You get 2 free deposits per day; each additional deposit costs 0.3 TRX.
* Your balance updates automatically in about 3 seconds.

**Verify your balance via API:**

```http
GET https://api.tronsave.io/v2/user-info
Headers: { "apikey": "YOUR_API_KEY" }
```

```json
{
  "error": false,
  "data": {
    "balance": "50000000",         // 50 TRX (unit: SUN — 1 TRX = 1,000,000 SUN)
    "depositAddress": "TKVSa...",  // Send TRX to this address to top up
    "representAddress": "TKVSa..." // Used as the "requester" field when placing orders
  }
}
```

## Step 2 — Estimate the cost

Before placing an order, call the estimate endpoint to see how much TRX the order will cost.

```http
POST https://api.tronsave.io/v2/estimate-buy-resource
Content-Type: application/json
```

```json
{
  "receiver": "YOUR_TRON_RECEIVER_ADDRESS",
  "resourceType": "ENERGY",
  "resourceAmount": 65000,
  "durationSec": 900,
  "unitPrice": "MEDIUM"
}
```

**Request fields:**

<table><thead><tr><th width="174">Field</th><th width="168">Example</th><th>Description</th></tr></thead><tbody><tr><td><code>receiver</code></td><td><code>"TFwUFW..."</code></td><td>The TRON address that will receive the Energy</td></tr><tr><td><code>resourceAmount</code></td><td><code>32000</code></td><td>Amount of Energy to buy (a single USDT TRC-20 transfer costs ~32,000 Energy)</td></tr><tr><td><code>durationSec</code></td><td><code>259200</code></td><td>Rental duration in seconds — <code>259200</code> = 3 days</td></tr><tr><td><code>unitPrice</code></td><td><code>"MEDIUM"</code></td><td>See the pricing table below</td></tr></tbody></table>

**Choosing `unitPrice`:**

<table><thead><tr><th width="177">Value</th><th>When to use</th><th>Price level</th></tr></thead><tbody><tr><td><code>"MEDIUM"</code></td><td>Default — suitable for most cases</td><td>Moderate</td></tr><tr><td><code>"FAST"</code></td><td>Need the order filled immediately</td><td>Higher</td></tr><tr><td><code>"SLOW"</code></td><td>Not time-sensitive, prioritize savings</td><td>Lowest</td></tr><tr><td><code>number</code> (SUN)</td><td>Full price control, e.g. <code>80</code></td><td>Custom</td></tr></tbody></table>

**Response:**

```json
{
  "error": false,
  "data": {
    "unitPrice": 64,
    "durationSec": 900,
    "estimateTrx": 4230000,    // Estimated cost: 4.23 TRX
    "availableResource": 65000 // Market currently has enough Energy available
  }
}
```

{% hint style="warning" %}
If **`availableResource`** If it is less than your requested **`resourceAmount`**, the market currently does not have enough Energy available to fill the order fully. See **`options.allowPartialFill`** in the next step.
{% endhint %}

## Step 3 — Create a buy order

Once you've confirmed the cost looks reasonable, create the order:

```http
POST https://api.tronsave.io/v2/buy-resource
Headers: { "apikey": "YOUR_API_KEY" }
Content-Type: application/json
```

```json
{
  "receiver": "YOUR_TRON_RECEIVER_ADDRESS",
  "resourceType": "ENERGY",
  "resourceAmount": 65000,
  "durationSec": 900,
  "unitPrice": "MEDIUM",
  "options": {
    "allowPartialFill": true,
    "preventDuplicateIncompleteOrders": true
  }
}
```

**Recommended `options` config:**

```jsonc
// For production bots (most common setup)
{
  "allowPartialFill": true,                 // Accept partial fill if market supply is low
  "preventDuplicateIncompleteOrders": true, // Prevent duplicate orders on retry
  "maxPriceAccepted": 100                   // Reject if unit price exceeds 100 SUN
}

// For urgent / must-fill-now orders
{
  "allowPartialFill": true,
  "onlyCreateWhenFulfilled": true
}
```

**Success response:**

```json
{
  "error": false,
  "data": {
    "orderId": "6818426a65fa8ea36d119999"
  }
}
```

Save the `orderId` — You'll need it in the next step.

## Step 4 — Check order status

```http
GET https://api.tronsave.io/v2/order/6818426a65fa8ea36d119999
Headers: { "apikey": "YOUR_API_KEY" }
```

```json
{
  "error": false,
  "data": {
    "id": "6818426a65fa8ea36d119999",
    "resourceAmount": 65000,
    "resourceType": "ENERGY",
    "fulfilledPercent": 100,   // 100 = order fully matched
    "remainAmount": 0,
    "price": 64,               // Actual unit price (SUN)
    "payoutAmount": 4230000,   // Total charged (SUN)
    "durationSec": 900,
    "delegates": [
      {
        "delegator": "THnnMC...", // Provider who delegated the Energy
        "amount": 65000,
        "txid": "19d3fa..."       // On-chain transaction ID
      }
    ]
  }
}
```

**Reading `fulfilledPercent`:**

<table><thead><tr><th width="164">Value</th><th>Meaning</th></tr></thead><tbody><tr><td><code>100</code></td><td>Order fully matched — Energy has been delegated</td></tr><tr><td><code>1–99</code></td><td>Partially matched (only possible when <code>allowPartialFill: true</code>)</td></tr><tr><td><code>0</code></td><td>Pending — waiting for a provider to match</td></tr></tbody></table>

{% hint style="success" %}
Energy is typically delegated within **a few seconds to a few minutes** after the order is matched.
{% endhint %}

## Full example (JavaScript)

A complete flow from estimate to order verification:

```javascript
const TRONSAVE_API = "https://api.tronsave.io";
const API_KEY = "YOUR_API_KEY";
const RECEIVER = "YOUR_TRON_RECEIVER_ADDRESS";

async function buyEnergy(amount = 32000, durationSec = 259200) {
  // Step 1: Estimate cost
  const estimateRes = await fetch(`${TRONSAVE_API}/v2/estimate-buy-resource`, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      receiver: RECEIVER,
      resourceType: "ENERGY",
      resourceAmount: amount,
      durationSec,
      unitPrice: "MEDIUM",
    }),
  });
  const { data: estimate } = await estimateRes.json();
  console.log(`Estimated cost: ${estimate.estimateTrx / 1e6} TRX`);

  // Step 2: Create a buy order
  const buyRes = await fetch(`${TRONSAVE_API}/v2/buy-resource`, {
    method: "POST",
    headers: {
      "apikey": API_KEY,
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      receiver: RECEIVER,
      resourceType: "ENERGY",
      resourceAmount: amount,
      durationSec,
      unitPrice: "MEDIUM",
      options: {
        allowPartialFill: true,
        preventDuplicateIncompleteOrders: true,
        maxPriceAccepted: 100,
      },
    }),
  });
  const { data: order } = await buyRes.json();
  console.log(`Order created: ${order.orderId}`);

  // Step 3: Wait and check order status
  await new Promise((r) => setTimeout(r, 5000)); // Wait 5 seconds

  const statusRes = await fetch(`${TRONSAVE_API}/v2/order/${order.orderId}`, {
    headers: { "apikey": API_KEY },
  });
  const { data: status } = await statusRes.json();

  if (status.fulfilledPercent === 100) {
    console.log(`Successfully delegated ${status.resourceAmount} Energy to ${RECEIVER}`);
  } else {
    console.log(`Order pending... ${status.fulfilledPercent}% filled`);
  }

  return status;
}

buyEnergy();
```

{% hint style="info" %}
**Prefer an SDK?** The [SDK](sdk.md) (TypeScript, Rust, Python, Java, PHP) is faster to get started:

```bash
npm install tronsave-sdk
```

This installs the TypeScript package; see the [SDK](sdk.md) page for the other languages.
{% endhint %}

## Test environment (Nile testnet)

Replace the base URL when testing:

<table><thead><tr><th width="124"></th><th width="268">Production</th><th>Testnet</th></tr></thead><tbody><tr><td><strong>Website</strong></td><td><code>https://tronsave.io</code></td><td><code>https://testnet.tronsave.io</code></td></tr><tr><td><strong>API</strong></td><td><code>https://api.tronsave.io</code></td><td><code>https://api-dev.tronsave.io</code></td></tr></tbody></table>

All endpoint paths remain the same — only the domain changes. See [Environments & Networks](environments.md) for details.

## Next steps

Now that you've placed your first order, you can:

* [**Extend an order**](api-reference/extend-orders/) — Renew the rental duration before it expires without creating a new order.
* [**View order history**](api-reference/buy-resources/api-key/order-history.md) — Track all orders under your account.
* [**Buy with Signed Transaction**](api-reference/buy-resources/signed-tx/) — If you prefer not to hold TRX inside TronSave.
* [**Full code examples**](code-examples/) — Complete working examples in JS, PHP, and Python.

***

_Need help? Reach out on_ [_Telegram_](https://t.me/tronsave) _or check the_ [_FAQ_](../resources/faq.md)_._
