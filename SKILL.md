---
name: clawcard
description: Email, SMS, virtual cards, crypto wallet, and credential vault for autonomous agents. Run clawcard agent commands to interact with your resources.
---

You have access to ClawCard via the `clawcard` CLI. All commands support `--json` for machine-readable output. Always pass `--json` when calling these commands.

## CRITICAL: Know Your Resources Before Acting

**BEFORE creating cards, sending USDC, or spending money, ALWAYS check what you already have:**

```
clawcard agent info --json        # Your identity, email, phone, wallet, budget
clawcard agent cards --json       # Your existing cards (check BEFORE creating new ones!)
clawcard agent wallet --json      # Your wallet (check BEFORE creating a new one!)
clawcard agent budget --json      # Your remaining FIAT budget
clawcard agent creds --json       # Your stored credentials
```

**You MUST list existing cards before every purchase.** If you already have an open `merchant_locked` card for the same merchant, REUSE IT — do not create a new one. Cards are limited and creating duplicates wastes budget.

---

## Email

List inbox:
```
clawcard agent emails --json [--limit 20] [--unread]
```

Send email:
```
clawcard agent emails send --to "recipient@example.com" --subject "Subject" --body "Body" --json
```

Mark as read:
```
clawcard agent emails read <email-id> --json
```

## SMS

List messages:
```
clawcard agent sms --json [--limit 20]
```

Send SMS:
```
clawcard agent sms send --to "+15551234567" --body "Message" --json
```

## Virtual Cards

### MANDATORY: Check existing cards FIRST

**Before EVERY payment, run this:**
```
clawcard agent cards --json
```

Look at the response. For each card, check:
- `status`: Is it `open`? (If `paused` or `closed`, skip it)
- `type`: Is it `merchant_locked`? (If so, it can be reused at the same merchant)
- `memo`: Does the description match the merchant/service you're paying?
- `spendLimitCents`: Does it have enough budget remaining?

**If you find an open merchant_locked card for the same merchant — USE IT. Do NOT create a new card.**

### Card types

- `single_use`: Auto-closes after one charge. Use for one-time purchases (domains, invoices, one-off payments). **This is the default.**
- `merchant_locked`: Locks to first merchant, allows repeat charges. Use ONLY when the user explicitly needs multiple purchases at the same merchant.

### Subscription warning

Cards CANNOT be topped up. If you are about to pay for a recurring subscription (monthly SaaS, hosting, etc.), STOP and warn the user: "This card has a fixed budget and can't be refilled — the renewal will fail when the budget runs out. Consider using a personal card for subscriptions."

### Commands

List cards:
```
clawcard agent cards --json
```

Create card (ONLY after confirming no existing card works):
```
clawcard agent cards create --amount <cents> --type <single_use|merchant_locked> --memo "description" --json
```

Get card details (PAN, CVV, expiry):
```
clawcard agent cards details <card-id> --json
```

Close card:
```
clawcard agent cards close <card-id> --json
```

## Stablecoin Wallet (USDC on Base)

Your agent can have a USDC wallet for crypto payments, agent-to-agent transfers, and x402 API access.

**Use wallet for:** x402 APIs (HTTP 402 responses), crypto transfers, micropayments
**Use cards for:** traditional merchant checkouts, websites with payment forms

### Setup

Check if you have a wallet:
```
clawcard agent wallet --json
```

If you get `"No wallet found"`, create one via the API or run `clawcard agent wallet` interactively.

### Commands

Check balance (shows USDC + FIAT spending power):
```
clawcard agent wallet balance --json
```

Send USDC to an address:
```
clawcard agent wallet send --to 0xADDRESS --amount 5.00 --json
```

Pay a URL via x402 (default — when a service returns HTTP 402):
```
clawcard agent wallet send --url https://api.example.com/data --json
```

Pay a URL via MPP (Machine Payments Protocol — Stripe/Tempo):
```
clawcard agent wallet send --url https://api.example.com/data --protocol mpp --json
```

View transaction history:
```
clawcard agent wallet transactions --json
```

If wallet balance is low, FIAT budget auto-converts to USDC transparently.

## Discover Services

**When you need a capability you don't have** (web search, data enrichment, image generation, etc.), search the x402 ecosystem:

```
clawcard agent discover --query "web search" --json
```

This returns paid API services you can access with your wallet. To use one:

```
clawcard agent wallet send --url https://discovered-service.com/endpoint --json
```

The wallet handles payment automatically via the x402 protocol. No API keys or subscriptions needed — just USDC micropayments.

Browse all available services:
```
clawcard agent discover --json
```

## On-Chain Identity (ERC-8004)

Your agent can have a verifiable on-chain identity — an NFT on Base that proves who the agent is, what it can do, and links to its wallet address.

Register or view identity:
```
clawcard agent identity --json
```

## A2A Agent Card

View your agent's discovery card (used by other agents to find and interact with you):
```
clawcard agent card --json
```

## Credentials

Store and retrieve secrets. Use consistent lowercase naming.

Naming convention:
- --service: lowercase service name (openai, aws, stripe, vercel, namecheap)
- --key: lowercase key type (api_key, secret_key, access_token, password)

List stored credentials:
```
clawcard agent creds --json
```

Store credential:
```
clawcard agent creds set --service <name> --key <key> --value <secret> --json
```

Retrieve credential:
```
clawcard agent creds get --service <name> --key <key> --json
```

## Budget

Check remaining budget:
```
clawcard agent budget --json
```

## Activity

View activity log:
```
clawcard agent activity --json [--limit 50]
```

## Billing Address

Get the user's billing address for checkout forms:
```
clawcard agent billing-address --json
```

Returns: name, line1, line2, city, state, zip, country.

## Paying on Checkout Pages

**FOLLOW THESE STEPS IN ORDER:**

1. **Check existing cards FIRST:**
   ```
   clawcard agent cards --json
   ```
   Look for an open card that matches this merchant. If found, skip to step 4.

2. **Check budget:**
   ```
   clawcard agent budget --json
   ```

3. **Create a card ONLY if no existing card works:**
   ```
   clawcard agent cards create --amount <cents> --type single_use --memo "description of purchase" --json
   ```

4. **Get billing address:**
   ```
   clawcard agent billing-address --json
   ```

5. **Fill non-payment fields** (name, email, address, zip) using the billing address.

6. **Get card details:**
   ```
   clawcard agent cards details <card-id> --json
   ```

7. **Fill card fields** (card number, expiry, CVC) yourself if possible.

8. **If card fields are in iframes you can't reach**, use the browser extension:
   ```
   clawcard agent pay --card-id <card-id> --json
   ```

9. **Verify fields are filled**, then submit the payment.

## Tips

- **ALWAYS check existing resources before creating new ones** — cards, wallet, credentials.
- Check budget before spending: `clawcard agent budget --json`
- For API payments that return HTTP 402, use the wallet: `clawcard agent wallet send --url <url> --json`
- For traditional checkouts, use cards. For crypto/x402, use the wallet.
- Default to `single_use` cards. Only use `merchant_locked` for repeat purchases at the same merchant.
- NEVER silently pay for a subscription. Always warn the user first.
- Store credentials with consistent naming so you can find them later.
- If you need a capability (search, enrichment, scraping, etc.), discover it: `clawcard agent discover --query "..." --json`
- If a service returns HTTP 402, your wallet can pay for it automatically.
