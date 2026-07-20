---
description: >-
  Buy Energy instantly by sending TRX to TronSave's bot address — a 1-hour
  rental order is created automatically from the amount you send.
---

# ZapBuy

**ZapBuy** is a fast, no-frills way to buy Energy: you **send TRX directly** to TronSave's default bot address, and the system automatically creates a **1-hour Energy rental order** sized to the amount of TRX received. No account, form, or extra steps required.

For the conceptual overview of all order types, see [Order Types](../../concepts/order-types.md).

## How it works

1. **Calculate the TRX to send** using the [Energy Calculator Tool](https://tronsave.io/tools/zapbuy). You can buy Energy flexibly based on your needs — you are not limited to fixed multiples.

<figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

2. **Send TRX** to TronSave's bot address:

```ini
TLx8h8fjv5pyuxCu292ZgjbU14XZSiLGg4
```

3. The system calculates how much Energy your TRX can rent.
4. A **1-hour rental order** is created automatically.
5. The Energy is delegated directly to the sender's wallet (if eligible to receive).

{% hint style="info" %}
* You must rent **at least 65,000 Energy**. Requests below 65,000 Energy are **ignored**, and **no order is created**.
*   ZapBuy only works when your order can be **matched immediately** with available Energy. If no match is found, you will not receive Energy.

    Contact TronSave with your transaction details to request a refund: [https://t.me/wantingtrx](https://t.me/wantingtrx)
{% endhint %}

## Technical details

<table><thead><tr><th width="211">Parameter</th><th>Value</th></tr></thead><tbody><tr><td>Supported Token</td><td><strong>TRX only</strong></td></tr><tr><td>Recipient TRX Address</td><td>TronSave bot address: <strong>TLx8h8fjv5pyuxCu292ZgjbU14XZSiLGg4</strong></td></tr><tr><td>Energy Recipient</td><td><strong>The wallet that sent the TRX</strong></td></tr><tr><td>Rental Duration</td><td><strong>1 hour</strong> (fixed)</td></tr><tr><td>Minimum Buy</td><td><strong>65,000 Energy</strong></td></tr><tr><td>Order Creation</td><td>Automatically upon receiving TRX</td></tr></tbody></table>

{% hint style="warning" %}
If your request comes out to less than **65,000 Energy**, no Energy is rented and your transaction is ignored.
{% endhint %}

## Frequently asked questions

**Q: Can I choose a rental duration other than 1 hour?**\
A: No. ZapBuy is designed for quick, fixed-duration rentals of 1 hour.

**Q: What happens if I send more TRX in one transaction?**\
A: The system still creates **a single order**, sized to the appropriate Energy amount.

**Q: I sent TRX but didn't receive Energy — why?**\
A: Check the following:

* You sent TRX from a **regular wallet**, not a **smart contract** or **exchange**.
* The amount you rent is at least **65,000 Energy**.
* Your transaction was **successfully confirmed on the TRON blockchain**.

If you have met all of the above and still did not receive Energy, contact us **immediately** at [https://t.me/wantingtrx](https://t.me/wantingtrx).

## Next steps

* [Order Types](../../concepts/order-types.md) · [Energy and Bandwidth](../../concepts/energy-and-bandwidth.md)
