---
description: The TronSave MCP server — drive TronSave authentication, orders, pricing, and delegate workflows from AI agents over Streamable HTTP.
---

# MCP Server

`tron-save-mcp-server` is a production-oriented [Model Context Protocol](https://modelcontextprotocol.io) server for the TronSave ecosystem. It exposes TronSave business operations as MCP tools over **Streamable HTTP** transport, with Redis-backed sessions and strong TypeScript + Zod contracts.

**Core capabilities:**

* Streamable MCP endpoint at `/mcp` (`POST`, `GET`, `DELETE`)
* Dual authentication model (`ApiKey` and `Signature`)
* Session and nonce security with Redis TTL
* Typed GraphQL and REST integrations
* Strict input/output schemas for all tools

## Mission

**TronSave Unified Resource Operations**

The server provides one MCP interface for platform and internal TronSave operations — authentication, account data, order lifecycle, pricing/estimation, and delegate extension workflows.

## Authentication model

The server supports two credential types:

* **`ApiKey`** — authenticates with your TronSave API key. Required for all **Internal Operations** tools.
* **`Signature`** — authenticates with a wallet-signed message. Required for some platform tools.

Call `tronsave_get_sign_message` to obtain a one-time message/nonce, sign it with your wallet, then call `tronsave_login` to create a server session.

{% hint style="info" %}
Internal tools always require an **API-key session**. Some platform tools require a **Signature session**. Tools marked `No` under **Requires Login** can be called immediately after MCP initialization without any login step.
{% endhint %}

See [Authentication](../authentication.md) for how to obtain an API key and choose between API-key and signed-transaction methods.

## Tool categories

### Platform authentication & identity

<table><thead><tr><th width="287">Tool</th><th width="146">Requires Login</th><th>Description</th></tr></thead><tbody><tr><td><code>tronsave_get_sign_message</code></td><td>No</td><td>Returns a one-time message/nonce to be signed by wallet for Signature login.</td></tr><tr><td><code>tronsave_login</code></td><td>No</td><td>Creates a server session using ApiKey or Signature credentials.</td></tr><tr><td><code>tronsave_user_info_get</code></td><td>Yes</td><td>Retrieves profile-level information for the authenticated platform user.</td></tr><tr><td><code>tronsave_user_permissions_get</code></td><td>Yes</td><td>Returns granted permissions/scopes for the current platform session.</td></tr><tr><td><code>tronsave_user_auto_setting_get</code></td><td>Yes</td><td>Gets current auto-settings related to platform user behavior.</td></tr></tbody></table>

### Platform market, orders & resource actions

<table><thead><tr><th width="218">Tool</th><th width="151">Requires Login</th><th>Description</th></tr></thead><tbody><tr><td><code>tronsave_order_book</code></td><td>No</td><td>Returns public buy/sell market depth and current order book data.</td></tr><tr><td><code>tronsave_orders_list</code></td><td>No</td><td>Lists platform orders for the current user/session context.</td></tr><tr><td><code>tronsave_order_detail</code></td><td>Yes</td><td>Fetches detailed information for one specific platform order.</td></tr><tr><td><code>tronsave_order_create</code></td><td>No</td><td>Creates an order using the legacy nested payload shape.</td></tr><tr><td><code>tronsave_order_create_onchain</code></td><td>No</td><td>Creates a new order using ONCHAIN payment mode and flat input.</td></tr><tr><td><code>tronsave_order_create_internal</code></td><td>No</td><td>Creates a new order using INTERNAL balance and flat input.</td></tr><tr><td><code>tronsave_order_create_pending</code></td><td>No</td><td>Creates a pending order that waits for deposit/settlement flow.</td></tr><tr><td><code>tronsave_order_update</code></td><td>No</td><td>Updates editable fields of an existing order.</td></tr><tr><td><code>tronsave_order_cancel</code></td><td>No</td><td>Cancels an open order when cancellation rules allow it.</td></tr><tr><td><code>tronsave_order_sell_manual</code></td><td>No</td><td>Triggers manual sell flow for eligible resource positions/orders.</td></tr><tr><td><code>tronsave_estimate_buy_resource</code></td><td>No</td><td>Estimates expected amount/cost before creating a buy order.</td></tr><tr><td><code>tronsave_energy_price_table_get</code></td><td>Yes</td><td>Returns pricing table snapshots for Energy/resource market.</td></tr><tr><td><code>tronsave_user_seller_energy_stats_get</code></td><td>Yes</td><td>Returns seller-side performance and Energy statistics.</td></tr><tr><td><code>tronsave_extendable_delegates_get</code></td><td>No</td><td>Lists delegate entries that can be extended.</td></tr></tbody></table>

### Internal operations

<table><thead><tr><th width="292">Tool</th><th width="144">Requires Login</th><th>Description</th></tr></thead><tbody><tr><td><code>tronsave_internal_account_get</code></td><td>Yes</td><td>Gets internal account/balance details for API-key session.</td></tr><tr><td><code>tronsave_get_deposit_address</code></td><td>Yes</td><td>Returns deposit address for internal funding workflow.</td></tr><tr><td><code>tronsave_internal_order_history</code></td><td>Yes</td><td>Lists internal order history records with filtering support.</td></tr><tr><td><code>tronsave_internal_order_details</code></td><td>Yes</td><td>Returns full details for an internal order record.</td></tr><tr><td><code>tronsave_internal_order_estimate</code></td><td>Yes</td><td>Calculates estimate for an internal order before submission.</td></tr><tr><td><code>tronsave_internal_order_create</code></td><td>Yes</td><td>Creates a new internal order using internal workflow parameters.</td></tr><tr><td><code>tronsave_internal_extend_delegates</code></td><td>Yes</td><td>Extends delegate duration/terms in internal workflow.</td></tr><tr><td><code>tronsave_internal_extend_request</code></td><td>Yes</td><td>Submits an extension request for internal delegate/order process.</td></tr></tbody></table>

## Connecting

The MCP server is hosted at `https://mcp.tronsave.io/mcp`. The package is published as `tron-save-mcp-server`.

The endpoint uses Streamable HTTP transport, accepting `POST`, `GET`, and `DELETE` requests. Point any MCP-compatible client (for example, an AI agent runtime) at the host endpoint:

```
https://mcp.tronsave.io/mcp
```

## Next steps

* [API Reference](README.md) · [Authentication](../authentication.md)
* [Buy Resources](buy-resources/README.md) · [Extend Orders](extend-orders/README.md)
