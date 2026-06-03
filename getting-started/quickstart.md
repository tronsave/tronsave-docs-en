---
description: Buy your first Energy in under 10 minutes — website or API.
---

# Quickstart

There are two fast paths to your first Energy purchase. Pick one.

## Path A — Buy on the website (no code)

1. Go to [tronsave.io/market](https://tronsave.io/market) and click **Connect** to connect your TRON wallet (e.g. TronLink).
2. Choose **Buy**, then set:
   * **Resource**: Energy (default) or Bandwidth
   * **Amount**: e.g. `32000` Energy (≈ one USDT TRC‑20 transfer)
   * **Duration**: e.g. 1 hour or 3 days
   * **Price tier**: `MEDIUM` is a good default — see [Order Types](../concepts/order-types.md)
3. Confirm and sign. Energy is delegated to your address within seconds once the order matches.

{% hint style="success" %}
That's it. To learn the different ways to buy (Normal, Pending, Smart, ZapBuy, Auto Buy), see [Buy Energy & Bandwidth](../guides/buy/README.md).
{% endhint %}

## Path B — Buy via the API (for developers)

In about 10 minutes you'll get an API key, deposit TRX, estimate cost, and place an order. Full walkthrough:

➡️ **[Developer Quickstart](../developers/quickstart.md)**

Minimal version:

```bash
# 1. Estimate cost
curl -X POST https://api.tronsave.io/v2/estimate-buy-resource \
  -H "Content-Type: application/json" \
  -d '{"receiver":"YOUR_TRON_ADDRESS","resourceType":"ENERGY","resourceAmount":65000,"durationSec":900,"unitPrice":"MEDIUM"}'

# 2. Create the order
curl -X POST https://api.tronsave.io/v2/buy-resource \
  -H "apikey: YOUR_API_KEY" -H "Content-Type: application/json" \
  -d '{"receiver":"YOUR_TRON_ADDRESS","resourceType":"ENERGY","resourceAmount":65000,"durationSec":900,"unitPrice":"MEDIUM","options":{"allowPartialFill":true,"preventDuplicateIncompleteOrders":true}}'
```

## Testing first?

Use the **Nile testnet** — same flow, no real TRX:

| | Production | Testnet (Nile) |
| --- | --- | --- |
| Website | `https://tronsave.io` | `https://testnet.tronsave.io` |
| API | `https://api.tronsave.io` | `https://api-dev.tronsave.io` |

## Next steps

* [Developer Quickstart](../developers/quickstart.md) · [Authentication](../developers/authentication.md) · [API Reference](../developers/api-reference/README.md)
* [How It Works](how-it-works.md) · [Order Types](../concepts/order-types.md)
