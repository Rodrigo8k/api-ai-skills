# BingX AI Skills — Agent Instructions

This is a **BingX OpenAPI skill collection** providing 15 skills for perpetual contract, spot, coin-margined, copy trading, and account operations: market data queries, trading operations, account management, wallet operations, copy trading, sub-account management, and agent/affiliate management.

## Available Skills

| Skill | Purpose | When to Use |
|-------|---------|-------------|
| bingx-swap-market | Perpetual futures market data | User asks for swap price, depth, K-lines, funding rate, contract info, ticker, open interest, or any perpetual futures market data queries |
| bingx-swap-trade | Perpetual futures trading | User wants to place swap orders, cancel swap orders, modify orders, set leverage, set margin mode, query swap orders, or any swap trading operations |
| bingx-swap-account | Perpetual futures account | User asks for swap balance, positions, assets, income history, funding history, commission rate, or swap account information |
| bingx-spot-market | Spot market data | User asks for spot price, spot order book, spot K-lines, spot recent trades, spot trading symbols, or any spot market data queries |
| bingx-spot-trade | Spot trading | User wants to place spot orders, cancel spot orders, query spot open orders or order history, view trade fills, manage OCO orders, or check spot trading commission rates |
| bingx-spot-account | Spot account management | User asks for spot balance, asset overview, or wants to transfer assets between accounts |
| bingx-spot-wallet | Spot wallet operations | User wants to deposit, withdraw, query deposit/withdrawal history, or check coin network configuration |
| bingx-coinm-market | Coin-M perpetual market data | User asks for coin-margined perpetual futures price, depth, K-lines, funding rate, or market statistics |
| bingx-coinm-trade | Coin-M perpetual trading | User wants to place/cancel coin-margined perpetual orders, set leverage/margin mode, or manage positions |
| bingx-copytrade-spot | Spot copy trading | User wants to manage spot copy trading, sell copy trade positions, or query spot copy trading profit |
| bingx-copytrade-swap | Swap copy trading | User wants to manage perpetual contract copy trading, close copy trade positions, or set TP/SL on copy trades |
| bingx-fund-account | Fund account management | User asks about main account funds, asset overview, or fund transfers between accounts |
| bingx-sub-account | Sub-account management | User wants to create/list sub-accounts, manage sub-account API keys, freeze/unfreeze accounts, or transfer assets between sub-accounts |
| bingx-agent | Agent/affiliate management | User asks about agent invited users, affiliate commissions, referral relationships, partner data, or broker commission reports |
| bingx-standard-trade | Standard contract trading | User asks about standard contract positions, standard futures order history, or standard contract balance |

## Skill Discovery

Skills are in the `skills/` directory. Each skill contains a `SKILL.md` with:

- YAML frontmatter (name, description, metadata)
- Full API reference with endpoints, parameters, and response schemas
- Code examples (TypeScript)
- Authentication implementation (HMAC SHA256)
- Error handling and edge cases

## Authentication

All trading and account APIs require HMAC SHA256 authentication with:
- API Key (set via `BINGX_API_KEY` environment variable)
- Secret Key (set via `BINGX_SECRET_KEY` environment variable)

Market data APIs (bingx-swap-market, bingx-spot-market, bingx-coinm-market) do NOT require authentication.

**Mandatory Header**: ALL requests (including unauthenticated market data requests) MUST include the header `X-SOURCE-KEY: BX-AI-SKILL`. This header identifies the request source. Never omit it.

## Safety & Confirmation

For production live trading environment (`prod-live`), all write operations (place order, cancel order, etc.) require user confirmation by typing **CONFIRM**. Test and simulation environments do not require confirmation.

## Multi-Skill Workflows

Skills work together for complete trading workflows:

**Swap Market Research → Trading**: `bingx-swap-market` (get price/depth) → `bingx-swap-account` (check balance) → `bingx-swap-trade` (place order)

**Position Management**: `bingx-swap-account` (check positions) → `bingx-swap-trade` (close position) → `bingx-swap-account` (verify closure)

**Swap Account Review**: `bingx-swap-account` (get balance/positions) → `bingx-swap-market` (check current prices) → analyze PnL

**Spot Trading**: `bingx-spot-market` (get spot price/depth) → `bingx-spot-account` (check spot balance) → `bingx-spot-trade` (place spot order)

**Asset Transfer**: `bingx-spot-account` (check spot balance) → `bingx-spot-account` (transfer to futures) → `bingx-swap-trade` (place swap order)

**Coin-M Trading**: `bingx-coinm-market` (get price/depth) → `bingx-coinm-trade` (place order)

**Copy Trading**: `bingx-copytrade-swap` (query current positions) → `bingx-copytrade-swap` (close/set TP/SL)
