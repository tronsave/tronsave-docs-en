---
description: >-
  The TronSave MCP server — price, buy, extend and sell TRON Energy and
  Bandwidth from AI agents over Streamable HTTP. Endpoints, login, all 29 tools
  and the main workflows.
---

# MCP Server

The TronSave [Model Context Protocol](https://modelcontextprotocol.io) server exposes the TronSave marketplace to AI agents as typed MCP tools. Every tool has strict input **and** output schemas and `readOnlyHint` / `destructiveHint` annotations, so agents and MCP clients can tell reads from writes.

| | |
|---|---|
| **Mainnet** | `https://mcp.tronsave.io/mcp` |
| **Testnet (Nile)** | `https://mcp.tronsave.io/testnet/mcp` |
| **Transport** | Streamable HTTP — `POST` for JSON-RPC, `DELETE` to end a session. `GET` returns `405` (no server-initiated stream). |
| **Protocol version** | `2025-06-18` |
| **Units** | Prices in **SUN** per resource unit per day (1 TRX = 1,000,000 SUN). Durations in **seconds**. |

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

Every tool except `tronsave_get_sign_message` and `tronsave_login` requires a logged-in session; without one it returns `SESSION_REQUIRED`. Logging in authenticates the **current** `mcp-session-id` (it does not change) for 24 hours — `tronsave_login` returns `expiresAt`.

{% tabs %}
{% tab title="Wallet signature" %}
Works for **every** tool, and is required for account, order and API-key management ("signature" in the tables below).

1. Call `tronsave_get_sign_message`. It returns `{ message, timestamp }`; `message` is the hex encoding of the timestamp.
2. Sign `message` client-side with the TRON wallet as a `signMessageV2` signature (for example `tronWeb.trx.signMessageV2(message)`, TronLink or WalletConnect).
3. Call `tronsave_login` with `{ "signature": "<signature>_<timestamp>" }`.
{% endtab %}

{% tab title="API key" %}
Works for the internal-balance tools and market data ("any" in the tables below), not for "signature" tools.

* Call `tronsave_login` with `{ "apiKey": "YOUR_API_KEY" }` — the raw key, no prefix.
* See [Authentication](../authentication.md) for how to get a key. With a signature session, `tronsave_generate_api_key` issues one and `tronsave_get_user_info` returns it in `internalAccount.apiKey`.
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
An `authorization: ApiKey …` HTTP header is only validated when the session starts — it does **not** log the session in. Always call `tronsave_login`. Never send a private key or seed phrase to the server: signing happens client-side. Treat the `mcp-session-id` like a credential; a leaked session acts as you until it expires.
{% endhint %}

## Tools

**Session**: `none` — no login; `any` — an API-key or a wallet-signature session; `signature` — a wallet-signature session only. **Effect** comes from the tool's annotations.

### Login & account

| Tool | Session | Effect | What it does |
|---|---|---|---|
| `tronsave_get_sign_message` | none | read-only | Issue a wallet-signable timestamp message helper for signature login. |
| `tronsave_login` | none | write | Authenticate with TronSave and create a server session. |
| `tronsave_get_user_info` | signature | read-only | Read the current authenticated user's profile and linked TronSave internal account. |
| `tronsave_get_user_permissions` | signature | read-only | Read the enabled permission operations (`autoSettings.permitOperations`) for the authenticated user. |
| `tronsave_get_user_auto_setting` | signature | read-only | Read the current user's auto-sell configuration (`autoSettings`). |

### Market data & pricing

| Tool | Session | Effect | What it does |
|---|---|---|---|
| `tronsave_get_min_price` | any | read-only | Quote the minimum unit price for a buy. |
| `tronsave_estimate_buy_resource` | any | read-only | Quote price and availability for buying ENERGY or BANDWIDTH for a receiver address before placing an order. |
| `tronsave_list_order_books` | any | read-only | Read market depth buckets for `ENERGY` or `BANDWIDTH`. |
| `tronsave_list_orders` | any | read-only | Query the order list with paging. |
| `tronsave_get_order` | signature | read-only | Read one order by `id` and return its full snapshot for NORMAL, FAST, or EXTEND order types. |
| `tronsave_list_extendable_delegates` | any | read-only | List extendable delegate candidates for a `receiver` and `resourceType` (ENERGY\|BANDWIDTH). |

### Orders (wallet signature)

| Tool | Session | Effect | What it does |
|---|---|---|---|
| `tronsave_create_order` | signature | write | Create a new buy order on the TronSave market. |
| `tronsave_update_order` | signature | write | Update an open order by `orderId` with partial fields (`receiver`, `newPrice`). |
| `tronsave_cancel_order` | signature | destructive | Cancel an open order by `orderId`. |
| `tronsave_sell_order_manual` | signature | destructive | Manually execute seller-side fulfillment of an existing order with a wallet `signedTx`. |

### Seller automation & API keys

| Tool | Session | Effect | What it does |
|---|---|---|---|
| `tronsave_register_auto_sell` | signature | write | Create the initial auto-sell configuration for the authenticated user. |
| `tronsave_update_auto_sell_setting` | signature | write | Update the existing auto-sell configuration with partial fields. |
| `tronsave_delete_auto_buy_setting` | signature | destructive | Permanently delete one auto-buy rule by `id` (`MObjectId`). |
| `tronsave_generate_api_key` | signature | destructive | Generate a new internal API key credential for the current user. |
| `tronsave_revoke_api_key` | signature | destructive | Revoke the caller's current internal API key. |

### Internal balance

| Tool | Session | Effect | What it does |
|---|---|---|---|
| `tronsave_get_internal_account` | any | read-only | Retrieve the TronSave internal account profile for the current session: represent address, deposit address, and balance (SUN). |
| `tronsave_get_deposit_address` | any | read-only | Fetches the specific deposit address for the TronSave internal account. |
| `tronsave_internal_order_book` | any | read-only | Return the current TronSave market depth/price tiers for ENERGY or BANDWIDTH via the api-key REST endpoint. |
| `tronsave_internal_order_estimate` | any | read-only | Estimate the TRX cost and availability for a buy order before submitting it (api-key internal account). |
| `tronsave_internal_order_create` | any | write | Place a new buy order for ENERGY or BANDWIDTH on TronSave, paid from the internal account balance. |
| `tronsave_internal_order_history` | any | read-only | List paginated order history for the internal account linked to the API key, newest first. |
| `tronsave_internal_order_details` | any | read-only | Fetch full details for one internal-account order by order ID. |
| `tronsave_internal_extend_delegates` | any | read-only | Return extendable delegations for a receiver plus an `extendData` payload for the extension flow. |
| `tronsave_internal_create_extend_request` | any | write | Submit an extension request for existing delegated resources on TronSave, paid from the internal account. |

`tronsave_*` platform tools use the TronSave GraphQL market. The internal-balance tools use the [REST API](README.md) against the TRX you deposited into your Internal Account.

## Workflows

**Price a purchase.** `tronsave_get_min_price` returns the lowest unit price; `tronsave_estimate_buy_resource` returns the unit price, the total `estimateTrx` (SUN) and the available amount; `tronsave_list_order_books` shows depth by price band.

**Buy from your Internal Account** (API-key or signature session):

1. `tronsave_get_internal_account` to check the balance. To top up, call `tronsave_get_deposit_address` (`amountTrx` ≥ 10) and send that TRX.
2. `tronsave_internal_order_estimate` immediately before ordering.
3. `tronsave_internal_order_create` with the estimated `unitPrice` (or `FAST` / `MEDIUM` / `SLOW`). It returns `{ orderId }`.
4. Follow it with `tronsave_internal_order_details` or `tronsave_internal_order_history`.

**Buy on the market** (signature session): `tronsave_estimate_buy_resource` for the same receiver, amount and duration, then `tronsave_create_order` with `paymentMethod`:

* `internal` — paid from the Internal Account;
* `onchain` — sign a TRX transfer of `estimateTrx` to the returned `systemDepositAddress` client-side and pass it as `paymentSignedTx`.

**Extend a delegation** (Internal Account):

1. `tronsave_internal_extend_delegates` with the receiver, `extendTo` (Unix **seconds**; milliseconds are rejected with `INVALID_EXTEND_TIME`) and `maxPriceAccepted`. It returns the extendable rows and `extendData`.
2. `tronsave_internal_create_extend_request` with that `extendData` unchanged.

{% hint style="info" %}
Prices and availability change with every TRON block (about 3 seconds). Re-run the estimate right before creating or updating an order, and never reuse an older quote. Before any write or destructive tool, show the user the receiver, amount, duration and TRX cost, and get their confirmation.
{% endhint %}

## Errors

Failed calls return `isError: true` with `{ error, message, details: { statusCode, code } }`:

| Error | Meaning |
|---|---|
| `SESSION_REQUIRED` (401) | Not logged in, or the session expired — call `tronsave_login` again. |
| "Platform tools require a signature session" | The session is API-key only — log in with a wallet signature. |
| `REST_ERROR` with `TSAS:<n> <NAME>` | Passed through from the TronSave API, e.g. `TSAS:107 INVALID_API_KEY`, `TSAS:220 ORDER_NOT_FOUND`. |
| MCP `-32602` Input validation error | The arguments don't match the tool's input schema; the message names the field. |

## For agents

* Agent skill (connection, login, tools, workflows): `https://tronsave.io/.well-known/agent-skills/tronsave-api/SKILL.md`
* MCP server card: `https://tronsave.io/.well-known/mcp/server-card.json`
* Agent authentication guide: `https://tronsave.io/auth.md`
* Server-hosted quickstart: `https://mcp.tronsave.io/quickstart`

## Next steps

* [API Reference](README.md) · [Authentication](../authentication.md)
* [Buy Resources](buy-resources/README.md) · [Extend Orders](extend-orders/README.md)
