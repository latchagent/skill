---
name: clawcard
description: Email, SMS, virtual cards, crypto wallet, and credential vault for autonomous agents. Run clawcard agent commands to interact with your resources.
---

You have access to ClawCard via the `clawcard` CLI. All commands support `--json` for machine-readable output. Always pass `--json` when calling these commands.

## Quick Start

```
clawcard agent info --json        # Your identity, email, phone, wallet, budget
clawcard agent cards --json       # Your existing cards
clawcard agent wallet --json      # Your wallet
clawcard agent budget --json      # Your remaining FIAT budget
clawcard agent creds --json       # Your stored credentials
clawcard agent catalog --json     # Browse all API capabilities
```

**ALWAYS check existing resources before creating new ones.** List cards before every purchase. Reuse open `merchant_locked` cards for the same merchant.

## Core Capabilities

| Capability | Command Reference |
|---|---|
| Email (send, receive, read) | [reference/email.md](reference/email.md) |
| SMS (send, receive) | [reference/sms.md](reference/sms.md) |
| Virtual Cards (create, pay, manage) | [reference/cards.md](reference/cards.md) |
| Crypto Wallet (USDC, ETH, pathUSD) | [reference/wallet.md](reference/wallet.md) |
| Credentials (store, retrieve secrets) | [reference/credentials.md](reference/credentials.md) |
| Checkout Payments (fill forms, pay) | [reference/checkout.md](reference/checkout.md) |
| Identity & Verification | [reference/identity.md](reference/identity.md) |

## Catalog — Paid API Capabilities

Your agent has access to paid API capabilities via the catalog. All calls use x402 payment via your wallet.

**How to call any capability:**
```
clawcard agent wallet send --url "https://clawcard.sh/api/catalog/<capability>" --method POST --body '<json>' --json
```

**Browse full catalog:** `clawcard agent catalog --json`

| Capability | Price | Reference |
|---|---|---|
| Social Data (15+ platforms) | $0.02-0.03 | [reference/catalog-social.md](reference/catalog-social.md) |
| Web Research (search, scrape, deep research) | $0.005-0.05 | [reference/catalog-research.md](reference/catalog-research.md) |
| GTM (keywords, competitor analysis) | $0.01-0.02 | [reference/catalog-research.md](reference/catalog-research.md) |
| Lead Gen (contacts, company enrichment) | $0.02-0.03 | [reference/catalog-research.md](reference/catalog-research.md) |
| Content (image gen, UGC, SEO) | $0.05-0.10 | [reference/catalog-content.md](reference/catalog-content.md) |

## Tips

- For API payments that return HTTP 402, use the wallet: `clawcard agent wallet send --url <url> --json`
- For traditional checkouts, use cards. For crypto/x402, use the wallet.
- Default to `single_use` cards. Only use `merchant_locked` for repeat purchases.
- NEVER silently pay for a subscription. Always warn the user first.
- If you need a capability, check the catalog first: `clawcard agent catalog --json`
- If a service returns HTTP 402, your wallet can pay for it automatically.
