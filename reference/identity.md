# Identity & Verification

## On-Chain Identity (ERC-8004)

Your agent has a verifiable on-chain identity — an NFT on Base that proves who the agent is.

Register or view identity:
```
clawcard agent identity --json
```

## A2A Agent Card

View your agent's discovery card (used by other agents to find you):
```
clawcard agent card --json
```

## World ID Verification

Verify your agent is backed by a real human using World ID.

```
clawcard agent verify
```

Check status:
```
clawcard agent verify --json
```

## Discover Services

Search the x402 ecosystem for paid APIs:
```
clawcard agent discover --query "web search" --json
```

Browse all:
```
clawcard agent discover --json
```

## Budget & Activity

Check budget:
```
clawcard agent budget --json
```

View activity log:
```
clawcard agent activity --json [--limit 50]
```

## Billing Address

```
clawcard agent billing-address --json
```

Returns: name, line1, line2, city, state, zip, country.
