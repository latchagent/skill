# Catalog: Scrape & Extract

## How to pay

```
clawcard agent pay <slug> '<json>' --json
```

## Capabilities

### scrape-url ($0.01) — powered by Firecrawl
Scrape a URL and get clean, structured content.
```json
{"url": "https://arxiv.org/abs/2307.06435"}
```
Optional: `formats`, `onlyMainContent`, `waitFor`

### crawl-site ($0.01, async) — powered by Firecrawl
Crawl an entire website and extract content from all pages.
```json
{"url": "https://docs.stripe.com", "limit": 10}
```
Returns `jobId` — poll with `clawcard agent jobs status <jobId> --json`

### extract-data ($0.01) — powered by Firecrawl
Extract structured data from a URL using a natural language prompt.
```json
{"urls": ["https://example.com/pricing"], "prompt": "Extract all pricing tiers"}
```

### web-contents ($0.01) — powered by Exa
Get full text, highlights, and summaries from URLs.
```json
{"ids": ["https://arxiv.org/abs/2307.06435"], "text": true}
```

Browse all scraping capabilities: `clawcard agent catalog-search "scrape" --json`
