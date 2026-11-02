---
description: The TronSave MCP server — 53 tools for quotes, orders, auto-buy and account operations, served over Streamable HTTP with OAuth 2.1 or API-key auth.
---

# MCP Server

TronSave runs a hosted [Model Context Protocol](https://modelcontextprotocol.io) server that exposes the TronSave market to AI agents: quotes, order creation and management, auto-buy and auto-sell, account and internal-balance operations. It is listed in the official MCP Registry as `io.github.tronsave/mcp`. Source: [github.com/tronsave/tronsave-mcp](https://github.com/tronsave/tronsave-mcp) (MIT).

{% hint style="info" %}
Setting up Claude Code, Cursor or Claude as a user? Follow [Use TronSave with AI Assistants](../../guides/ai-assistants.md).
{% endhint %}

## Endpoints

All endpoints speak **Streamable HTTP** (`POST`, `GET`, `DELETE`). The network is fixed by the URL.

<table><thead><tr><th width="330">URL</th><th width="110">Network</th><th>Auth</th></tr></thead><tbody><tr><td><code>https://mcp.tronsave.io/mcp</code></td><td>Mainnet</td><td>Optional: public tools need none; send an API key header or log in with a tool. Also accepts OAuth tokens.</td></tr><tr><td><code>https://mcp.tronsave.io/oauth/mcp</code></td><td>Mainnet</td><td>OAuth 2.1 required (401 with <code>WWW-Authenticate</code> until the user signs in).</td></tr><tr><td><code>https://mcp.tronsave.io/testnet/mcp</code></td><td>Nile</td><td>As <code>/mcp</code>.</td></tr><tr><td><code>https://mcp.tronsave.io/oauth/testnet/mcp</code></td><td>Nile</td><td>As <code>/oauth/mcp</code>.</td></tr></tbody></table>

Discovery: `/.well-known/mcp.json`, `/llms.txt`, `/.well-known/oauth-authorization-server`, and protected-resource metadata per endpoint (RFC 9728).

## Authentication

<table><thead><tr><th width="190">Method</th><th>How</th><th>Unlocks</th></tr></thead><tbody><tr><td><strong>None</strong></td><td>Connect to <code>/mcp</code></td><td>Public tools: prices, quotes, order book, market data, on-chain orders paid by a wallet-signed transaction.</td></tr><tr><td><strong>OAuth 2.1</strong></td><td>Connect to <code>/oauth/mcp</code>. Dynamic client registration and PKCE S256; the user signs in on a TronSave page with an API key or a wallet signature. Access tokens 1 h; refresh tokens rotate, 30 days for API-key grants and 24 h for wallet grants.</td><td>API-key tools and/or wallet tools, depending on how the user signed in.</td></tr><tr><td><strong>API key</strong></td><td>Header <code>Authorization: ApiKey &#x3C;key></code>, or the <code>tronsave_login</code> tool with <code>apiKey</code></td><td>Internal-balance tools. See <a href="../authentication.md#get-an-api-key">Get an API key</a>.</td></tr><tr><td><strong>Wallet signature</strong></td><td><code>tronsave_get_sign_message</code> → sign the message in the wallet → <code>tronsave_login</code> with <code>&#x3C;signature>_&#x3C;timestamp></code>. Valid for 24 h.</td><td>Wallet-account tools: orders, auto-sell, auto-buy, notifications, API keys.</td></tr></tbody></table>

`tronsave_get_session_status` reports what the current session can do. Each tool's requirement is also in `tools/list` as `_meta["io.tronsave/auth"]` (`public` | `session` | `signature-session`).

## Tools

Every tool carries MCP annotations (`readOnlyHint`, `destructiveHint`, `idempotentHint`), so clients ask before writes. Errors return `{"error":{"code","message","retryable","hint"}}`. Writes that spend funds are never retried automatically.

### Session

`tronsave_get_session_status`, `tronsave_get_sign_message`, `tronsave_login`, `tronsave_logout` — public.

### Market and quotes (public)

`tronsave_get_market_overview`, `tronsave_get_price_table`, `tronsave_get_system_info`, `tronsave_get_min_price`, `tronsave_list_order_books`, `tronsave_estimate_buy_resource`, `tronsave_suggest_create_order`, `tronsave_get_account_resources`, `tronsave_list_orders`, `tronsave_list_extendable_delegates`, `tronsave_list_extendable_receivers`, `tronsave_list_flash_unstake_orders`, `tronsave_estimate_flash_unstake`.

### Buy

`tronsave_create_order` — public for on-chain payment (the user's wallet signs the payment transaction); uses the internal balance when signed in with an API key.

### Orders, account and seller (wallet signature)

`tronsave_get_order`, `tronsave_update_order`, `tronsave_cancel_order`, `tronsave_sell_order_manual`, `tronsave_get_user_info`, `tronsave_get_user_permissions`, `tronsave_get_user_auto_setting`, `tronsave_register_auto_sell`, `tronsave_update_auto_sell_setting`, `tronsave_get_seller_stats`, `tronsave_list_delegations`, `tronsave_list_internal_transactions`, `tronsave_get_flash_unstake_request`, `tronsave_generate_api_key`, `tronsave_revoke_api_key`.

### Auto-buy and bulk (wallet signature)

`tronsave_list_auto_buy_settings`, `tronsave_create_auto_buy_setting`, `tronsave_update_auto_buy_setting`, `tronsave_delete_auto_buy_setting`, `tronsave_bulk_create_internal_orders`.

### Notifications and referral (wallet signature)

`tronsave_list_notifications`, `tronsave_mark_notifications_read`, `tronsave_get_referral`, `tronsave_get_telegram_notify_setting`, `tronsave_update_telegram_notify_setting`, `tronsave_get_agency_bot`.

### Internal balance (API key)

`tronsave_get_internal_account`, `tronsave_get_deposit_address`, `tronsave_internal_order_book`, `tronsave_internal_order_estimate`, `tronsave_internal_order_create`, `tronsave_internal_order_history`, `tronsave_internal_order_details`, `tronsave_internal_extend_delegates`, `tronsave_internal_create_extend_request`.

Full input and output schemas for every tool: [TOOLS.md](https://github.com/tronsave/tronsave-mcp/blob/main/docs/TOOLS.md).

## Units

Unit prices are SUN per resource unit and payment amounts are SUN (1 TRX = 1,000,000 SUN). `durationSec` is seconds. From one day up, a rental is charged per started day; use `estimateTrx` from `tronsave_estimate_buy_resource` for the exact total. `extendTo` in the internal extend tools is a Unix timestamp in seconds.

## Connect from code

```bash
# Claude Code
claude mcp add --transport http tronsave https://mcp.tronsave.io/oauth/mcp
```

```json
{ "mcpServers": { "tronsave": { "url": "https://mcp.tronsave.io/oauth/mcp" } } }
```

The JSON form works in Cursor and other `mcpServers` clients. Snippets for the Anthropic and OpenAI APIs, raw HTTP and the full OAuth details are in the [integration quickstart](https://mcp.tronsave.io/quickstart).

## Next steps

* [Use TronSave with AI Assistants](../../guides/ai-assistants.md) · [Authentication](../authentication.md)
* [Buy Resources](buy-resources/README.md) · [Extend Orders](extend-orders/README.md)
