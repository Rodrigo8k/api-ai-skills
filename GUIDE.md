# BingX API Skills User Guide

> A quick-start guide for AI coding assistant users

---

## Overview

This repository is a collection of BingX exchange API skills designed for **AI coding assistants**. It includes 15 skill modules covering perpetual futures, spot trading, coin-margined contracts, copy trading, and account management.

Once installed, you can interact with the AI using natural language to query market data, place trades, and manage accounts — no manual API coding required.

**Supported AI Assistants**: Claude Code, Cursor, and other AI coding assistants that support the skills mechanism.

---

## Installation

### Universal Installation (Claude Code / Cursor / Others)

```bash
npx skills add BingX-API/api-ai-skills
```

### Claude Code (with auto-update support)

```bash
/plugin add BingX-API/api-ai-skills
```

> For detailed installation instructions, see [README.github.md](README.github.md)

---

## Skill Catalog

### USDT-M Perpetual Futures

| Skill | Purpose | Auth Required |
|-------|---------|---------------|
| `bingx-swap-market` | Market data: price, depth, klines, funding rate, open interest, etc. | No |
| `bingx-swap-trade` | Trading: place/cancel orders, set leverage, margin mode, etc. | Yes |
| `bingx-swap-account` | Account: balance, positions, fee rates, fund flow, etc. | Yes |

### Spot Trading

| Skill | Purpose | Auth Required |
|-------|---------|---------------|
| `bingx-spot-market` | Market data: price, depth, klines, recent trades, etc. | No |
| `bingx-spot-trade` | Trading: place/cancel orders, order queries, OCO orders, etc. | Yes |
| `bingx-spot-account` | Account: balance, asset overview, asset transfers, etc. | Yes |
| `bingx-spot-wallet` | Wallet: deposits, withdrawals, transfer records, etc. | Yes |

### Coin-M Perpetual Futures

| Skill | Purpose | Auth Required |
|-------|---------|---------------|
| `bingx-coinm-market` | Market data: price, depth, klines, funding rate, etc. | No |
| `bingx-coinm-trade` | Trading: place/cancel orders, leverage, margin mode, etc. | Yes |

### Copy Trading

| Skill | Purpose | Auth Required |
|-------|---------|---------------|
| `bingx-copytrade-spot` | Spot copy trading: sell positions, query profit, etc. | Yes |
| `bingx-copytrade-swap` | Swap copy trading: close positions, set TP/SL, commissions, etc. | Yes |

### Account Management

| Skill | Purpose | Auth Required |
|-------|---------|---------------|
| `bingx-fund-account` | Fund account: balance, asset overview, inter-account transfers, etc. | Yes |
| `bingx-sub-account` | Sub-accounts: create/freeze accounts, manage API keys, etc. | Yes |
| `bingx-agent` | Agent/broker: invited users, commissions, partner data, etc. | Yes |

### Standard Contract

| Skill | Purpose | Auth Required |
|-------|---------|---------------|
| `bingx-standard-trade` | Standard contract: positions, order history, balance, etc. | Yes |

---

## Authentication

### Skills That Do NOT Require Auth

The following market data skills can be used **without an API key**:

- `bingx-swap-market` (perpetual futures market data)
- `bingx-spot-market` (spot market data)
- `bingx-coinm-market` (coin-margined futures market data)

### Skills That Require Auth

All other trading and account skills require the following environment variables:

- `BINGX_API_KEY` — Your API Key
- `BINGX_SECRET_KEY` — Your Secret Key

Create your API Key at [BingX User Center → API Management](https://bingx.com/en/accounts/api).

### Environments

| Environment | Description | Primary URL | Fallback URL |
|-------------|-------------|-------------|--------------|
| `prod-live` | Production (real funds) | `https://open-api.bingx.com` | `https://open-api.bingx.pro` |
| `prod-vst` | Simulated trading (testnet) | `https://open-api-vst.bingx.com` | `https://open-api-vst.bingx.pro` |

> For full authentication technical details, see [skills/references/authentication.md](skills/references/authentication.md)

---

## Safety Mechanisms

### Write Operation Confirmation

In the `prod-live` (production) environment, all write operations (placing orders, canceling orders, transfers, etc.) require you to type **CONFIRM** before execution. The simulated environment does not require confirmation.

### Key Masking

The AI automatically masks sensitive keys when displaying them:

- API Key: shows first 5 + last 4 characters, e.g. `su1Qc...8akf`
- Secret Key: fully masked, shows only last 5 characters, e.g. `***...aws1`

---

## Usage Examples

After installation, you can talk to the AI in natural language. Here are some common examples:

### Market Queries

```
What is the current price of BTC-USDT?
```

```
Show me the 1-hour klines for ETH-USDT
```

```
What is the current funding rate for BTC-USDT?
```

### Trading Operations

```
Place a market buy order for 0.01 BTC-USDT
```

```
Place a limit sell order for ETH-USDT at price 4000, quantity 0.1
```

```
Cancel all open orders for BTC-USDT
```

### Account Management

```
Show my futures account balance and positions
```

```
Check my spot account balance
```

```
Transfer 100 USDT from spot to futures account
```

### Other Operations

```
List my sub-accounts
```

```
Show my agent invited users
```

---

## Multi-Skill Workflows

Multiple skills can be combined for complete trading workflows:

### Futures Trading Flow

```
swap-market (check BTC-USDT price and depth)
    ↓
swap-account (check balance and available margin)
    ↓
swap-trade (place order)
```

### Position Management Flow

```
swap-account (check current positions and PnL)
    ↓
swap-market (check latest price)
    ↓
swap-trade (close or adjust position)
    ↓
swap-account (verify closure)
```

### Spot Trading Flow

```
spot-market (query spot price and depth)
    ↓
spot-account (check spot balance)
    ↓
spot-trade (place buy or sell order)
```

### Cross-Account Asset Transfer

```
spot-account (check spot balance)
    ↓
spot-account / fund-account (transfer assets to futures account)
    ↓
swap-trade (trade with transferred funds)
```

---

## File Structure

Each skill module consists of two files:

```
skills/<skill-name>/
├── SKILL.md           # Agent behavior instructions
└── api-reference.md   # Complete API documentation
```

- **`SKILL.md`**: Defines the skill's purpose, quick reference table, TypeScript code examples, and agent interaction rules (authentication, confirmation flow, etc.)
- **`api-reference.md`**: Full API endpoint documentation including parameters, response fields, example JSON, and error codes

Shared reference documents are in the [skills/references/](skills/references/) directory:

- [authentication.md](skills/references/authentication.md) — HMAC SHA256 signing implementation
- [base-urls.md](skills/references/base-urls.md) — Environment base URL configuration
