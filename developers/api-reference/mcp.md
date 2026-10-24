---
description: >-
  The TronSave MCP server — price, buy, extend and sell TRON Energy and
  Bandwidth from AI agents over Streamable HTTP. Endpoints, login, all 53 tools
  and the main workflows.
---

# MCP Server

The TronSave [Model Context Protocol](https://modelcontextprotocol.io) server exposes the TronSave marketplace to AI agents as typed MCP tools. Every tool has strict input **and** output schemas and `readOnlyHint` / `destructiveHint` annotations, so agents and MCP clients can tell reads from writes. It also serves a guide resource (`tronsave://guide/agent`) and two prompts (`buy_resource`, `account_overview`).

| | |
|---|---|
| **Mainnet** | `https://mcp.tronsave.io/mcp` |
| **Testnet (Nile)** | `https://mcp.tronsave.io/testnet/mcp` |
| **Transport** | Streamable HTTP — `POST` for JSON-RPC, `DELETE` to end a session. `GET` returns `405` (no server-initiated stream). |
| **Protocol version** | `2025-06-18` |
| **Units** | Prices in **SUN** per resource unit per day (1 TRX = 1,000,000 SUN). Durations in **seconds**; `extendTo` is a Unix time in **seconds**. |

The network is fixed by the URL. Use the testnet endpoint (Nile TRX, no real value) to try the write tools.

## Connect

Point any MCP client at the endpoint. No headers are needed to connect:

```json
{
  "mcpServers": {
    "tronsave": { "url": "https://mcp.tronsave.io/mcp" }
  }
}
```

The `initialize` response carries an `mcp-session-id` header, which MCP clients resend on every request automatically.

## Log in

Market data, quotes, order lists and on-chain-paid orders need **no login**. The other tools need an API key or a wallet signature; the **Login** column below says which. MCP clients can read the same value from `tools/list` as `_meta["io.tronsave/auth"]` (`public`, `session` = API key, `signature-session` = wallet signature). `tronsave_get_session_status` shows the current login at any time.

{% tabs %}
{% tab title="API key" %}
Unlocks the internal-balance tools and `paymentMethod: "internal"`.

* Call `tronsave_login` with `{ "apiKey": "YOUR_API_KEY" }` (the raw key, no prefix), **or**
* configure the client to send `Authorization: ApiKey YOUR_API_KEY` (or `x-api-key: YOUR_API_KEY`) on every request. No login call is needed then.

See [Authentication](../authentication.md) for how to get a key. With a signature login, `tronsave_generate_api_key` issues one.
{% endtab %}

{% tab title="Wallet signature" %}
Unlocks profile, own orders, auto-buy and auto-sell rules, seller data, notifications, referral, Telegram alerts, bulk buy and API-key management.

1. Call `tronsave_get_sign_message`. It returns `{ message, timestamp }`.
2. Sign `message` in the user's TRON wallet as a `signMessageV2` signature (for example `tronWeb.trx.signMessageV2(message)`, TronLink or WalletConnect).
3. Call `tronsave_login` with `{ "signature": "<signature>_<timestamp>" }`.

If the wallet has an internal account, its API key is loaded as well, so the API-key tools work in the same session.
{% endtab %}
{% endtabs %}

A login is bound to the current `mcp-session-id` for 24 hours (`expiresAt`); `tronsave_logout` removes it.

{% hint style="warning" %}
Never send a private key or seed phrase to the server: all signing happens in the user's wallet. Treat the `mcp-session-id` and API keys as credentials; a leaked session acts as you until it expires.
{% endhint %}

## Tools

**Login**: `none` — no login; `api-key` — an internal API key (login, header, or a signature login of a wallet with an internal account); `signature` — a wallet-signature login. **Effect** comes from the tool's annotations.

### Session

| Tool | Login | Effect | What it does |
|---|---|---|---|
| `tronsave_get_sign_message` | none | read-only | Return the message the wallet signs for signature login. |
| `tronsave_login` | none | write | Log this MCP session in with an API key or a wallet signature. |
| `tronsave_logout` | none | write | Remove the login from this MCP session. |
| `tronsave_get_session_status` | none | read-only | Show the current login (none, API key or signature), its expiry and which tool groups it unlocks. |

### Market data & pricing (no login)

| Tool | Login | Effect | What it does |
|---|---|---|---|
| `tronsave_get_min_price` | none | read-only | Minimum unit price (SUN) for an amount and duration. |
| `tronsave_estimate_buy_resource` | none | read-only | Unit price, total cost (SUN) and available amount for a buy. |
| `tronsave_get_price_table` | none | read-only | Price grid: unit price for each amount × duration. |
| `tronsave_list_order_books` | none | read-only | Market depth by price band. |
| `tronsave_suggest_create_order` | none | read-only | Cheapest way to fill a buy: new order, top-up, or extending existing delegations. |
| `tronsave_get_market_overview` | none | read-only | Energy/bandwidth available and listed, pooled sellers, seller APY. |
| `tronsave_get_system_info` | none | read-only | TronSave and TRON parameters: minimum amounts, burn rates, system deposit address. |
| `tronsave_get_account_resources` | none | read-only | Staked and delegatable energy/bandwidth and TRX balance of any address. |
| `tronsave_list_orders` | none | read-only | Market orders, paged. |
| `tronsave_list_extendable_delegates` | none | read-only | Delegations of a receiver that can be extended (planning). |
| `tronsave_list_extendable_receivers` | none | read-only | Receivers with extendable delegations bought by an address. |
| `tronsave_list_flash_unstake_orders` | none | read-only | Flash-unstake market: available, ongoing or completed orders. |
| `tronsave_estimate_flash_unstake` | none | read-only | What an address would receive by flash-unstaking now. |

### Orders

| Tool | Login | Effect | What it does |
|---|---|---|---|
| `tronsave_create_order` | none | write | Create a buy order, paid on-chain (no login) or from the internal balance (API key). |
| `tronsave_get_order` | signature | read-only | One order's full snapshot. |
| `tronsave_update_order` | signature | write | Change an open order's receiver or price. |
| `tronsave_cancel_order` | signature | destructive | Cancel an open order. |
| `tronsave_sell_order_manual` | signature | destructive | Fill an order as a seller with a signed delegation transaction. |
| `tronsave_bulk_create_internal_orders` | signature | write | Buy the same amount for many receivers from the internal balance. |

### Internal balance (API key)

| Tool | Login | Effect | What it does |
|---|---|---|---|
| `tronsave_get_internal_account` | api-key | read-only | Internal account: balance (SUN), represent and deposit address. |
| `tronsave_get_deposit_address` | api-key | read-only | Deposit address for topping up the internal balance. |
| `tronsave_internal_order_book` | api-key | read-only | Market depth via the REST API. |
| `tronsave_internal_order_estimate` | api-key | read-only | Cost and availability of a buy paid from the internal balance. |
| `tronsave_internal_order_create` | api-key | write | Buy energy/bandwidth paid from the internal balance. |
| `tronsave_internal_order_history` | api-key | read-only | Orders of the internal account, newest first. |
| `tronsave_internal_order_details` | api-key | read-only | One internal-account order. |
| `tronsave_internal_extend_delegates` | api-key | read-only | Extendable delegations plus the `extendData` for an extend request. |
| `tronsave_internal_create_extend_request` | api-key | write | Extend delegations, paid from the internal balance. |
| `tronsave_list_internal_transactions` | signature | read-only | Internal balance ledger: deposits, payments, refunds. |

### Auto-buy

| Tool | Login | Effect | What it does |
|---|---|---|---|
| `tronsave_list_auto_buy_settings` | signature | read-only | Auto-buy rules. |
| `tronsave_create_auto_buy_setting` | signature | write | Create a rule that buys when a receiver's resource runs low. |
| `tronsave_update_auto_buy_setting` | signature | write | Pause, resume or change an auto-buy rule. |
| `tronsave_delete_auto_buy_setting` | signature | destructive | Delete an auto-buy rule. |

### Seller & auto-sell

| Tool | Login | Effect | What it does |
|---|---|---|---|
| `tronsave_get_seller_stats` | signature | read-only | Seller totals and daily earnings. |
| `tronsave_list_delegations` | signature | read-only | Delegations the seller sent to buyers. |
| `tronsave_get_user_auto_setting` | signature | read-only | Auto-sell configuration. |
| `tronsave_register_auto_sell` | signature | write | Create the auto-sell configuration. |
| `tronsave_update_auto_sell_setting` | signature | write | Change the auto-sell configuration. |
| `tronsave_get_user_permissions` | signature | read-only | Permission operations enabled for auto-sell. |

### Account, alerts & API keys

| Tool | Login | Effect | What it does |
|---|---|---|---|
| `tronsave_get_user_info` | signature | read-only | Profile and linked internal account. |
| `tronsave_list_notifications` | signature | read-only | Notifications, newest first, with the unread count. |
| `tronsave_mark_notifications_read` | signature | write | Mark one or all notifications read. |
| `tronsave_get_referral` | signature | read-only | Referral code, sponsor, referrals and bonus. |
| `tronsave_get_telegram_notify_setting` | signature | read-only | Telegram alert filters. |
| `tronsave_update_telegram_notify_setting` | signature | write | Replace the Telegram alert filters (needs a linked Telegram account). |
| `tronsave_get_agency_bot` | signature | read-only | The user's Telegram agency (resale) bot and its rewards. |
| `tronsave_get_flash_unstake_request` | signature | read-only | The user's flash-unstake request and payout progress. |
| `tronsave_generate_api_key` | signature | destructive | Issue a new internal API key. |
| `tronsave_revoke_api_key` | signature | destructive | Revoke the current internal API key. |

`tronsave_*` platform tools use the TronSave GraphQL market. The internal-balance tools use the [REST API](README.md) against the TRX you deposited into your Internal Account.

## Workflows

**Price a purchase** (no login). `tronsave_get_min_price` returns the lowest unit price; `tronsave_estimate_buy_resource` returns the unit price, the total `estimateTrx` (SUN) and the available amount. `tronsave_get_price_table` gives the amount × duration grid, `tronsave_list_order_books` the depth by price band, and `tronsave_suggest_create_order` whether a new order, a top-up or extending existing delegations is cheapest.

**Buy from your Internal Account** (API key):

1. `tronsave_get_internal_account` to check the balance. To top up, call `tronsave_get_deposit_address` (`amountTrx` ≥ 10) and send that TRX.
2. `tronsave_internal_order_estimate` immediately before ordering.
3. `tronsave_internal_order_create` with the estimated `unitPrice` (or `FAST` / `MEDIUM` / `SLOW`). It returns `{ orderId }`.
4. Follow it with `tronsave_internal_order_details` or `tronsave_internal_order_history`.

**Buy on the market**: `tronsave_estimate_buy_resource` for the same receiver, amount and duration, then `tronsave_create_order` with `paymentMethod`:

* `onchain` (no login) — have the user sign a TRX transfer of `estimateTrx` to the returned `systemDepositAddress` and pass it as `paymentSignedTx`;
* `internal` (API key) — paid from the Internal Account.

**Buy for many receivers** (signature): `tronsave_bulk_create_internal_orders` buys the same amount for each receiver from the Internal Account. `paymentAmount` must equal the total payout (±2%).

**Keep a wallet topped up** (signature): `tronsave_create_auto_buy_setting` buys `resourceAmount` whenever the receiver's resource drops below `thresholdResourceAmount` (at least 100,000), spending up to `limitBudgetInSun` (more than 10 TRX, covered by the Internal Account balance). Each receiver can have one active rule. Manage rules with `tronsave_list_auto_buy_settings`, `tronsave_update_auto_buy_setting` (`isActive` pauses or resumes) and `tronsave_delete_auto_buy_setting`.

**Extend a delegation** (API key, paid from the Internal Account):

1. `tronsave_internal_extend_delegates` with the receiver, `extendTo` (Unix **seconds**; milliseconds are rejected with `INVALID_EXTEND_TIME`) and `maxPriceAccepted`. It returns the extendable rows and `extendData`.
2. `tronsave_internal_create_extend_request` with that `extendData` unchanged.

**Sell** (signature): `tronsave_get_seller_stats` and `tronsave_list_delegations` for earnings; `tronsave_get_user_auto_setting`, then `tronsave_register_auto_sell` or `tronsave_update_auto_sell_setting` for auto-sell. `tronsave_get_account_resources` (no login) shows what an address can delegate.

{% hint style="info" %}
Prices and availability change with every TRON block (about 3 seconds). Re-run the estimate right before creating or updating an order, and never reuse an older quote. Before any write or destructive tool, show the user the receiver, amount, duration and TRX cost, and get their confirmation.
{% endhint %}

## Errors

Failed calls return `isError: true` with the text `{"error":{"code","message","retryable","hint"}}`. Follow `hint`. Never repeat a write when `retryable` is `false`; after `UPSTREAM_TIMEOUT` on a write, check the state (for example with `tronsave_get_order`) before trying again.

| Code | Meaning | What to do |
|---|---|---|
| `SESSION_REQUIRED` | The tool needs a login | Log in as the hint says, then retry |
| `INVALID_SESSION_TYPE` | The tool needs a wallet signature; the session is API-key only | Log in with a wallet signature |
| `API_KEY_SESSION_REQUIRED` | The tool needs an internal API key | Log in with an API key or send the header |
| `INVALID_API_KEY`, `INVALID_SIGNATURE`, `SIGNATURE_EXPIRED` | Credentials rejected | Fix the key or sign again |
| `UPSTREAM_UNAUTHORIZED` | TronSave rejected the session | Log in again |
| `GRAPHQL_ERROR`, `REST_ERROR` | A business rule rejected the request, e.g. `TSAS:220 ORDER_NOT_FOUND` | Read the message and fix the inputs |
| `UPSTREAM_TIMEOUT`, `UPSTREAM_UNREACHABLE`, `UPSTREAM_UNAVAILABLE` | TronSave is slow or down | Reads: retry later. Writes: check the state first |

An "Input validation error" means the arguments don't match the tool's input schema; the message names the field.

## For agents

* Agent skill (connection, login, tools, workflows): `https://tronsave.io/.well-known/agent-skills/tronsave-api/SKILL.md`
* MCP server card: `https://tronsave.io/.well-known/mcp/server-card.json`
* Agent authentication guide: `https://tronsave.io/auth.md`
* Server-hosted quickstart: `https://mcp.tronsave.io/quickstart`
* Server discovery: `https://mcp.tronsave.io/.well-known/mcp.json`, `https://mcp.tronsave.io/llms.txt`

## Next steps

* [API Reference](README.md) · [Authentication](../authentication.md)
* [Buy Resources](buy-resources/README.md) · [Extend Orders](extend-orders/README.md)
