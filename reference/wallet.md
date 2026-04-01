# Stablecoin Wallet (USDC + pathUSD on Base)

Your agent has a wallet on Base with three balances:

- **ETH** — gas fees (needed for on-chain transactions)
- **USDC** — x402 protocol payments and direct transfers
- **pathUSD** — MPP (Machine Payments Protocol) payments via Tempo/Stripe

**Use wallet for:** x402 APIs (HTTP 402 responses), MPP services, crypto transfers, micropayments
**Use cards for:** traditional merchant checkouts, websites with payment forms

## Commands

Check wallet:
```
clawcard agent wallet --json
```

Check balance (ETH, USDC, pathUSD):
```
clawcard agent wallet balance --json
```

Send USDC to an address:
```
clawcard agent wallet send --to 0xADDRESS --amount 5.00 --json
```

Pay a URL via x402 (when a service returns HTTP 402):
```
clawcard agent wallet send --url https://api.example.com/data --json
```

Pay a URL via MPP (uses pathUSD):
```
clawcard agent wallet send --url https://api.example.com/data --protocol mpp --json
```

View transaction history:
```
clawcard agent wallet transactions --json
```

## Funding

Fund wallet interactively (wizard with ETH/USDC/pathUSD options):
```
clawcard agent wallet fund
```

Fund via JSON:
```
clawcard agent wallet fund --amount 10.00 --json
```

## Bridge USDC to pathUSD

For MPP services, you need pathUSD. The fund wizard's "MPP services" option bridges USDC to pathUSD on Tempo.

Explorer links: x402 → Basescan, MPP/bridge → Tempo Explorer.
