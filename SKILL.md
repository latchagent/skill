---
name: clawcard
description: Email, SMS, virtual cards, and credential vault for autonomous agents. Run clawcard agent commands to interact with your resources.
---

You have access to ClawCard via the `clawcard` CLI. All commands support `--json` for machine-readable output. Always pass `--json` when calling these commands.

## Step 1: Check your identity

```
clawcard agent info --json
```

Returns your agent name, email address, phone number, and remaining budget.

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

IMPORTANT: Always list cards first. Reuse open merchant_locked cards for the same merchant.

Card types (REQUIRED — you must specify one):
- single_use: auto-closes after one charge. Use for one-time purchases (domains, invoices).
- merchant_locked: locks to first merchant, allows repeat charges. Use for subscriptions (hosting, SaaS).

List cards:
```
clawcard agent cards --json
```

Create card:
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

## Tips

- Always run `clawcard agent info --json` first to verify your identity.
- Check budget before creating cards: `clawcard agent budget --json`
- Reuse merchant_locked cards for repeat purchases at the same merchant.
- Use single_use cards for one-time purchases — they auto-close after one charge.
- Store credentials in the vault with consistent naming so you can find them later.
