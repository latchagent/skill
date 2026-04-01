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

## Stablecoin Wallet (USDC + pathUSD on Base)

Your agent has a wallet on Base with three balances:

- **ETH** — gas fees (needed for on-chain transactions)
- **USDC** — x402 protocol payments and direct transfers
- **pathUSD** — MPP (Machine Payments Protocol) payments via Tempo/Stripe

**Use wallet for:** x402 APIs (HTTP 402 responses), MPP services, crypto transfers, micropayments
**Use cards for:** traditional merchant checkouts, websites with payment forms

### Setup

Check if you have a wallet:
```
clawcard agent wallet --json
```

If you get `"No wallet found"`, create one via the API or run `clawcard agent wallet` interactively.

### Commands

Check balance (shows ETH, USDC, and pathUSD balances):
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

Pay a URL via MPP (Machine Payments Protocol — Stripe/Tempo, uses pathUSD):
```
clawcard agent wallet send --url https://api.example.com/data --protocol mpp --json
```

**Explorer links:** Payment responses include explorer URLs. x402 transactions link to Basescan (`basescan.org/tx/...`), MPP and bridge transactions link to Tempo Explorer (`explore.tempo.xyz/receipt/...`).

View transaction history:
```
clawcard agent wallet transactions --json
```

### Fund Wizard (3 options)

The fund command is a wizard with three funding options:

1. **Gas fees (ETH)** — opens Coinbase Onramp to buy ETH (needed for on-chain transactions)
2. **x402 services (USDC)** — opens Coinbase Onramp to buy USDC (for x402 protocol payments)
3. **MPP services (pathUSD)** — bridges USDC from your existing balance to pathUSD on Tempo (for MPP payments)

Fund your wallet interactively (runs the wizard):
```
clawcard agent wallet fund
```

Fund via JSON (opens Coinbase Onramp for USDC/ETH):
```
clawcard agent wallet fund --amount 10.00 --json
```

### Bridge USDC to pathUSD

To pay for MPP services, you need pathUSD. The fund wizard's "MPP services" option converts USDC from your wallet balance to pathUSD on Tempo. This is a bridge operation (USDC on Base -> pathUSD on Tempo).

The bridge response includes both a Basescan link (Base transaction) and a Tempo Explorer link (Tempo receipt).

Or send USDC directly to your wallet address on Base. Check your address with `clawcard agent wallet --json`.

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

## World ID Verification

Verify your agent is backed by a real human using World ID. Services that support World AgentKit will trust your agent more — free trials, lower spam scores, priority access.

```
clawcard agent verify
```

Requires the World App on your phone with a verified World ID. Check status:
```
clawcard agent verify --json
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

## Catalog — Included Capabilities

Your agent has access to paid API capabilities included with ClawCard. All calls use x402 payment via your wallet.

Browse the catalog:
```
clawcard agent catalog --json
```

### How to call a catalog capability

All catalog capabilities are called via wallet send:
```
clawcard agent wallet send --url "https://clawcard.sh/api/catalog/<capability>" --method POST --body '<json>' --json
```

The endpoint handles x402 payment automatically. Your wallet is charged the listed price per request.

### Available Capabilities

**Research:**
- `web-search` ($0.005) — Search the web. Body: `{"query": "..."}`
- `scrape-url` ($0.02) — Extract content from a URL. Body: `{"url": "..."}`
- `deep-research` ($0.05) — In-depth research. Body: `{"topic": "..."}`

**GTM:**
- `keyword-research` ($0.01) — Keyword opportunities. Body: `{"keyword": "..."}`
- `competitor-analysis` ($0.02) — Competitor analysis. Body: `{"domain": "..."}`

**Lead Gen:**
- `find-contacts` ($0.03) — Find contacts. Body: `{"company": "..."}`
- `enrich-company` ($0.02) — Company data. Body: `{"domain": "..."}`

**Content:**
- `generate-image` ($0.05, async) — Generate images. Body: `{"prompt": "..."}`
- `generate-ugc` ($0.10, async) — Generate UGC video. Body: `{"prompt": "..."}`
- `seo-content` ($0.05) — SEO content. Body: `{"keyword": "..."}`

**Ops:**
- `enrich-prospect` ($0.02) — Prospect enrichment. Body: `{"email": "..."}`

**Social Data (live — 15+ platforms):**
- `social-profile` ($0.02) — Get a creator's profile (followers, bio, stats). Body: `{"platform": "tiktok", "handle": "..."}`
- `social-posts` ($0.03) — Get recent posts/videos. Body: `{"platform": "instagram", "handle": "..."}`
- `social-search` ($0.02) — Search content by keyword. Body: `{"platform": "youtube", "query": "..."}`
- `social-post` ($0.02) — Get details on a specific post/video. Body: `{"url": "https://tiktok.com/..."}` (platform auto-detected from URL)
- `social-comments` ($0.02) — Get comments on a post. Body: `{"url": "https://youtube.com/watch?v=..."}` (platform auto-detected)
- `social-transcript` ($0.03) — Get video transcript. Body: `{"url": "https://youtube.com/watch?v=..."}` (platform auto-detected)

Platforms: tiktok, instagram, youtube, twitter/x, linkedin, facebook, reddit, threads, bluesky, snapchat, truthsocial, pinterest, twitch, kick, google

For URL-based capabilities (social-post, social-comments, social-transcript), the platform is auto-detected from the URL domain. You can also pass `"platform"` explicitly to override.

For full catalog details with all supported platforms per capability, run: `clawcard agent catalog --json`

### Async capabilities

Some capabilities (generate-image, generate-ugc) are async. They return a `jobId` instead of immediate results:

```json
{"jobId": "job_abc123", "status": "pending"}
```

Poll for completion:
```
clawcard agent jobs status <job-id> --json
```

When complete, download the artifact:
```
clawcard agent artifacts download <art-id> --json
```

## Artifacts

List artifacts:
```
clawcard agent artifacts list --json
```

Download artifact:
```
clawcard agent artifacts download <art-id> --json
```

## Jobs

List async jobs:
```
clawcard agent jobs list --json
```

Check job status:
```
clawcard agent jobs status <job-id> --json
```
