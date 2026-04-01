# Virtual Cards

## MANDATORY: Check existing cards FIRST

**Before EVERY payment, run:**
```
clawcard agent cards --json
```

Look for an open card matching the merchant. If found, reuse it — do NOT create a new card.

## Card Types

- `single_use`: Auto-closes after one charge. Default for one-time purchases.
- `merchant_locked`: Locks to first merchant, allows repeat charges. Only for explicit repeat-purchase needs.

## Subscription Warning

Cards CANNOT be topped up. For recurring subscriptions, warn the user: "This card has a fixed budget — the renewal will fail when the budget runs out."

## Commands

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
