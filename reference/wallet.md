# Crypto Wallet (Advanced)

> Most agents don't need this. Use `clawcard agent pay` for catalog capabilities and `clawcard billing topup` for balance. The wallet is for advanced crypto use cases only.

Your agent has a USDC wallet on Base for direct crypto transfers and on-chain interactions.

## Commands

Check wallet:
```
clawcard agent wallet --json
```

Send USDC to an address:
```
clawcard agent wallet send --to 0xADDRESS --amount 5.00 --json
```

View transaction history:
```
clawcard agent wallet transactions --json
```

Fund wallet:
```
clawcard agent wallet fund --amount 10.00 --json
```
