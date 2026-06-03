---
description: Standardized terminology used across the TronSave docs.
---

# Glossary

| Term | Definition |
| --- | --- |
| **Energy** | TRON resource consumed to execute smart contracts (e.g. USDT TRC‑20 transfers). Obtained by staking TRX or renting on TronSave. |
| **Bandwidth** | TRON resource consumed by transaction byte size. Every transaction needs some. |
| **TRX** | The native token of the TRON network. |
| **SUN** | The smallest unit of TRX. **1 TRX = 1,000,000 SUN.** All prices are quoted in SUN. |
| **Stake 2.0** | TRON's staking (freeze) mechanism that produces Energy/Bandwidth. The basis of TronSave's supply. |
| **Delegation** | Granting Energy/Bandwidth from one account to another on‑chain. A filled order results in a delegation to the `receiver`. |
| **Provider / Seller** | A user who stakes TRX and rents out (delegates) the resulting Energy to earn APY. |
| **Buyer** | A user who rents Energy/Bandwidth from the marketplace. |
| **Order book** | The live set of buy/sell orders that determines the market price. |
| **Internal Account** | A TronSave‑managed balance you deposit TRX into; the API can spend from it automatically. Tied to an API Key. |
| **API Key** | A secret identifier linked to your Internal Account, used to authorize API requests. |
| **Signed Transaction** | An alternative auth method where you sign a TRX payment from your own wallet per purchase (full custody). |
| **`unitPrice`** | Price per resource unit in SUN, or a tier: `SLOW` / `MEDIUM` / `FAST`. |
| **`resourceType`** | `ENERGY` or `BANDWIDTH`. Defaults to `ENERGY`. |
| **`durationSec`** | Rental duration in seconds. Default `259200` (3 days). |
| **`receiver`** | The TRON address that receives the rented resource. |
| **`requester`** | The address placing/representing the order (the `representAddress` of your internal account). |
| **`fulfilledPercent`** | Order fill status: `0` pending, `1–99` partial, `100` complete. |
| **APR / APY** | Provider returns. See [Pricing & APY](pricing-and-apy.md) for formulas. |
| **WTRX** | Wrapped TRX, used in some [SDK](../developers/sdk/wtrx.md) payment flows. |
| **ZapBuy** | Buy Energy by sending TRX directly to a bot address; fixed 1‑hour rental. |
| **Fast Charge** | API flow for rapid Energy charging (estimate → create → track → confirm). |
| **Nile** | The TRON testnet TronSave uses (`api-dev.tronsave.io`). |

> Terminology standard: write **Energy** and **Bandwidth** capitalized when referring to the TRON resources; **TRX** and **SUN** in caps; field names in `code`.
