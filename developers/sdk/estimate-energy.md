---
description: Estimate the Energy a contract call will consume before you send it.
---

# Estimate Energy

Before broadcasting a contract call, you can ask TronSave to estimate how much Energy it will consume and what it would cost to rent that Energy on the marketplace. Use `estimateV2()` exactly like `send()` — pass the same parameters, but get an estimate back instead of broadcasting the transaction.

## Usage

The call signature is identical to `send()`. To estimate a request, replace `send()` with `estimateV2()`:

```tsx
// Example: method().send()

const onSend = async () => {
  try {
    const instance = await tronSave.contract().at("...");
    const testSend = await instance.method().send({
      feeLimit: 1e9,
      callValue: 1000000,
      ...
    });
  } catch (err) {
    console.log(err);
  }
};

// With estimateV2()

const onEstimateEnergy = async () => {
  try {
    const instance = await tronSave.contract().at("...");
    const testEstimateEnergy = await instance.method().estimateV2({
      feeLimit: 1e9,
      callValue: 1000000,
      ...
    });
    console.log(testEstimateEnergy);
    // Response type: {
    //   available_energy: 0,
    //   discount_percent: 0,
    //   total_estimate_energy: 0,
    //   buy_energy_price: 0
    // }
  } catch (err) {
    console.log(err);
  }
};
```

## Response

{% tabs %}
{% tab title="Result estimateV2()" %}
```typescript
{
  available_energy: 976304,
  discount_percent: 80,
  total_estimate_energy: 976304,
  buy_energy_price: 65,
}
```
{% endtab %}
{% endtabs %}

<table><thead><tr><th width="227">Keys</th><th width="99">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>available_energy</code></td><td>number</td><td>The amount of Energy the system can provide for the request above. If it meets 100%, it returns the same amount of Energy as estimated.</td></tr><tr><td><code>discount_percent</code></td><td>number</td><td>The percentage saved compared to burning TRX. For example, <code>80</code> means renting this Energy is about 80% cheaper than the on-chain burn cost.</td></tr><tr><td><code>total_estimate_energy</code></td><td>number</td><td>The amount of Energy the system has estimated for the command.</td></tr><tr><td><code>buy_energy_price</code></td><td>number</td><td>The Energy price (SUN) at which you can buy the entire estimated amount of Energy on the TronSave market.</td></tr></tbody></table>

{% hint style="info" %}
`buy_energy_price` is quoted in SUN per unit of Energy. 1 TRX = 1,000,000 SUN. See the [Glossary](../../concepts/glossary.md) for resource and pricing terminology.
{% endhint %}

## Next steps

- Learn how Energy is consumed in [Energy & Bandwidth](../../concepts/energy-and-bandwidth.md).
- Review [Pricing & APY](../../concepts/pricing-and-apy.md) to understand market prices.
