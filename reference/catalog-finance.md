# Catalog: Finance & Markets

## How to pay

```
clawcard agent pay <slug> '<json>' --json
```

## Capabilities

### stock-quote ($0.01) — powered by Alpha Vantage
Get the latest price, volume, and change for a stock.
```json
{"symbol": "AAPL"}
```

### sec-filings ($0.01) — powered by EDGAR
Get a company's SEC filing history — 10-Ks, 10-Qs, 8-Ks.
```json
{"ticker": "AAPL"}
```

More finance capabilities available — intraday/daily/weekly time series, forex, crypto, commodities, technical indicators, exchange rates.

Browse all: `clawcard agent catalog-search "finance" --json`
