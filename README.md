# BingX OpenAPI Skills

> A BingX Exchange API Skills Library for AI coding assistants, providing complete API support for perpetual contracts and spot trading

---

## Quick Start

### General Installation

Use the `npx skills add` command to install this skill package in various AI coding assistants:

```bash
npx skills add BingX-API/api-ai-skills
```

This method supports:
- ✅ Claude Code
- ✅ Cursor
- ✅ Other AI assistants that support the skills mechanism

---

### Claude Code Exclusive Features

This project provides enhanced support for **Claude Code**

#### 🚀 Quick Installation

```bash
/plugin marketplace add BingX-API/api-ai-skills
/plugin install bingx-ai-skills
```

### Usage After Installation

After installation, the AI assistant will automatically recognize the following skills:

**USDT-M Perpetual Contracts**:
- **bingx-swap-market** — Query perpetual contract market data (price, depth, K-lines, funding rate, open interest, etc.)
- **bingx-swap-trade** — Perform perpetual contract trading operations (place orders, cancel orders, leverage settings, margin mode, etc.)
- **bingx-swap-account** — Query account information (balance, positions, commission rates, fund flow, etc.)

**Spot Trading**:
- **bingx-spot-market** — Query spot market data (price, depth, K-lines, trade records, etc.)
- **bingx-spot-trade** — Perform spot trading operations (place orders, cancel orders, order queries, OCO orders, etc.)
- **bingx-spot-account** — Query spot account information (balance, asset overview, asset transfers, etc.)
- **bingx-spot-wallet** — Wallet operations (deposits, withdrawals, transfer records, etc.)

**Coin-M Perpetual Contracts**:
- **bingx-cswap-market** — Query coin-margined contract market data
- **bingx-cswap-trade** — Coin-margined contract trading operations

**Copy Trading**:
- **bingx-copytrade-spot** — Spot copy trading
- **bingx-copytrade-swap** — Contract copy trading

**Account Management**:
- **bingx-fund-account** — Fund account management
- **bingx-sub-account** — Sub-account management
- **bingx-agent** — Agent account operations

You can communicate with the AI directly in natural language, for example:
```
Query the current price of BTC-USDT
```
```
Place a limit buy order for BTC-USDT at price 50000, quantity 0.01
```
```
Check my account balance and positions
```
```
Query depth data for spot ETH-USDT
```

---

## Project Overview

This repository is a collection of Skills designed for **AI coding assistants**, enabling AI to directly understand and call [BingX Exchange](https://bingx.com)'s REST API without users having to manually write request code.

### Skills Mechanism

Each skill module consists of two files:

- **`SKILL.md`** — Agent behavior instructions
  - Module purpose and quick reference table
  - TypeScript example code
  - Agent interaction rules (authentication, confirmation mechanisms, etc.)

- **`api-reference.md`** — Complete API documentation
  - Parameter descriptions and response fields
  - Example JSON and error codes

After reading these files, AI can autonomously complete the entire workflow including HMAC SHA256 signing, request construction, response parsing, etc., allowing users to query market data, place trades, manage accounts, and more through natural language.

### Feature Coverage

The current version provides complete support for **Perpetual Contracts (Swap)** and **Spot Trading**:

- **Perpetual Contracts**: Market data, trading operations, account management
- **Spot Trading**: Market data, order management, account assets
- **Coin-M Contracts**: Market data, trading interface
- **Copy Trading**: Spot and contract copy trading
- **Account Management**: Fund accounts, sub-accounts, agent accounts

---

## Project Features

### Implemented Modules

#### Swap Market (`bingx-swap-market`)

No authentication required, provides perpetual contract public market data:

| Feature | API Endpoint |
|---------|--------------|
| Contract Information | `GET /openApi/swap/v2/quote/contracts` |
| Order Book Depth | `GET /openApi/swap/v2/quote/depth` |
| Recent Trades | `GET /openApi/swap/v2/quote/trades` |
| Mark Price / Index Price | `GET /openApi/swap/v2/quote/premiumIndex` |
| Funding Rate | `GET /openApi/swap/v2/quote/fundingRate` |
| K-line Data | `GET /openApi/swap/v2/quote/klines` |
| Open Interest | `GET /openApi/swap/v2/quote/openInterest` |
| 24h Ticker Statistics | `GET /openApi/swap/v2/quote/ticker` |
| Best Bid/Ask Price | `GET /openApi/swap/v2/quote/bookTicker` |
| Historical Trades | `GET /openApi/swap/v2/quote/historicalTrades` |
| Mark Price K-lines | `GET /openApi/swap/v2/quote/markPriceKlines` |
| Latest Price | `GET /openApi/swap/v2/quote/price` |
| Trading Rules | `GET /openApi/swap/v2/quote/tradingRules` |

#### Swap Trade (`bingx-swap-trade`)

Requires HMAC SHA256 authentication, supports complete perpetual contract trading operations:

- Place order / Test order / Batch orders (supports `quoteOrderQty` for USDT amount-based ordering)
- Cancel order / Batch cancel / Cancel all open orders
- Cancel and replace order / Batch cancel and replace (atomic operation)
- Modify order quantity
- Cancel All After (scheduled cancel all)
- Query order details
- Set leverage
- Switch margin mode (cross / isolated / isolated multi-position)
- Switch position mode (one-way / hedge)

#### Swap Account (`bingx-swap-account`)

Requires HMAC SHA256 authentication, provides account information queries:

- Account balance query (v3 API)
- Current positions query
- Commission rate query
- Fund flow query and export

---

### Directory Structure Convention

Each sub-module follows a unified dual-file structure (see [`rules/bingx-skill-convention.mdc`](rules/bingx-skill-convention.mdc) for details):

```
skills/
├── SKILL.md                    # Root skill entry (aggregates all modules)
├── references/                 # Common reference documentation
│   └── SKILL.md                # HMAC SHA256 authentication implementation guide
├── swap-market/                # USDT-M perpetual market data
│   └── SKILL.md
├── swap-trade/                 # USDT-M perpetual trading
│   └── SKILL.md
├── swap-account/               # USDT-M perpetual account
│   └── SKILL.md
├── spot-market/                # Spot market data
│   └── SKILL.md
├── spot-trade/                 # Spot trading
│   └── SKILL.md
├── spot-account/               # Spot account
│   └── SKILL.md
├── spot-wallet/                # Spot wallet
│   └── SKILL.md
├── cswap-market/               # Coin-M contract market data
│   └── SKILL.md
├── cswap-trade/                # Coin-M contract trading
│   └── SKILL.md
├── copytrade-spot/             # Spot copy trading
│   └── SKILL.md
├── copytrade-swap/             # Contract copy trading
│   └── SKILL.md
├── fund-account/               # Fund account
│   └── SKILL.md
├── sub-account/                # Sub-account
│   └── SKILL.md
└── agent/                      # Agent account
    └── SKILL.md
```

---

### Runtime Environment

See [`skills/references/base-urls.md`](skills/references/base-urls.md) for the full environment table and TypeScript helper.

---

### Safety Mechanisms

- **Write Operation Confirmation**: In the `prod-live` (production live) environment, all write operations (placing orders, canceling orders, etc.) require the user to actively type **CONFIRM** before execution; testnet and testing environments do not require confirmation.
- **Key Display Rules**:
  - API Key: Display first 5 characters + last 4 characters, e.g., `su1Qc...8akf`
  - Secret Key: Fully masked, only display last 5 characters, e.g., `***...aws1`

---
