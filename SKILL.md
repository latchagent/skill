---
name: clawcard
description: Email, SMS, virtual cards, and credential vault for autonomous agents. Gives your agent a real email inbox, US phone number, virtual Mastercards with spend limits, and encrypted credential storage.
---

You have access to **ClawCard** — a platform that gives you a real email address, phone number, virtual debit cards, and an encrypted credential vault.

## Authentication

All requests use your API key as a Bearer token:

```
Authorization: Bearer $CLAWCARD_API_KEY
```

Base URL: `https://www.clawcard.sh`

## Step 1: Discover Your Identity

**Always call this first.** It tells you your keyId, email, phone, and budget — everything you need for subsequent calls.

```
GET /api/me
```

Response:
```json
{
  "keyId": "agt_abc123",
  "name": "my-agent",
  "email": "inbox-7xk@mail.clawcard.sh",
  "phone": "+12025551234",
  "spendLimitCents": 5000
}
```

Use the `keyId` as `KEY_ID` in all endpoints below.

## Email

**Read inbox:**
```
GET /api/agents/KEY_ID/emails?limit=20&unread=true
```

Query params: `limit` (1-100, default 20), `unread` (true/false), `from` (filter by sender), `subject_contains` (search subject).

**Send email:**
```
POST /api/agents/KEY_ID/emails/send
Content-Type: application/json

{"to": "recipient@example.com", "subject": "Hello", "body": "Message text"}
```

**Mark as read:**
```
POST /api/agents/KEY_ID/emails/EMAIL_ID/read
```

## SMS

Your key has a dedicated US phone number for receiving SMS (e.g., verification codes).

**Read received messages:**
```
GET /api/agents/KEY_ID/sms?limit=50
```

**Send SMS:**
```
POST /api/agents/KEY_ID/sms/send
Content-Type: application/json

{"to": "+15551234567", "body": "Hello from my agent!"}
```

## Virtual Cards

You can create virtual Mastercards with per-card spend limits.

### Card Types (REQUIRED — you must specify one)

- **`single_use`** — Auto-closes after one successful transaction. Use for one-time purchases (buying a domain, paying an invoice).
- **`merchant_locked`** — Locks to the first merchant that charges it, allows repeat charges from that merchant. Use for subscriptions and recurring payments (hosting, SaaS tools).

### Important: Check existing cards first

**Before creating a new card, always list existing cards.** If there's an open `merchant_locked` card for the same merchant, reuse it — this doesn't count against the monthly card limit.

**List cards:**
```
GET /api/agents/KEY_ID/cards
```

Response includes all cards with `type`, `status`, and `spendLimitCents`. Look for cards with `status: "open"`.

**Create a card:**
```
POST /api/agents/KEY_ID/cards
Content-Type: application/json

{"amountCents": 2000, "type": "single_use", "memo": "Domain purchase"}
```

The `type` field is **required**. Must be `"single_use"` or `"merchant_locked"`.

Response includes full card details: `pan`, `cvv`, `exp_month`, `exp_year`.

**Get card details (PAN, CVV, expiry):**
```
GET /api/agents/KEY_ID/cards/CARD_ID
```

Use this to retrieve card details for any open card, including reusing a `merchant_locked` card.

**Close a card:**
```
PATCH /api/agents/KEY_ID/cards/CARD_ID
Content-Type: application/json

{"action": "close"}
```

Actions: `"pause"`, `"resume"`, or `"close"` (permanent).

## Credentials Vault

Encrypted key-value storage for API keys and secrets you need at runtime.

**List stored credentials (names only):**
```
GET /api/agents/KEY_ID/credentials
```

**Store a credential:**
```
POST /api/agents/KEY_ID/credentials
Content-Type: application/json

{"service": "aws", "key": "access_key", "value": "AKIA..."}
```

**Retrieve a credential:**
```
GET /api/agents/KEY_ID/credentials/SERVICE/KEY
```

Example: `GET /api/agents/KEY_ID/credentials/openai/api_key`

## Budget

Budget controls how much you can spend on virtual cards.

**Check remaining budget:**
```
GET /api/agents/KEY_ID/budget
```

**Allocate budget (moves funds from account balance to this key):**
```
POST /api/agents/KEY_ID/budget
Content-Type: application/json

{"amountCents": 2000}
```

## Activity Log

Full audit trail of everything you've done.

```
GET /api/agents/KEY_ID/activity?limit=50
```

## Tips

- Always call `GET /api/me` first to discover your identity and available resources.
- Check your budget before creating cards — `GET /api/agents/KEY_ID/budget`.
- Reuse `merchant_locked` cards for repeat purchases at the same merchant.
- Use `single_use` cards for one-time purchases — they auto-close after one charge.
- Store credentials in the vault rather than hardcoding them.
