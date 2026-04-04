# Catalog: Search

## How to pay

```
clawcard agent pay <slug> '<json>' --json
```

## Capabilities

### web-search ($0.01) — powered by Exa
AI-powered web search with neural, auto, or deep modes.
```json
{"query": "Latest developments in LLM capabilities"}
```
Optional: `type`, `numResults`, `category`, `includeDomains`, `excludeDomains`, `startPublishedDate`, `endPublishedDate`

### web-search-brave ($0.04) — powered by Brave
Independent, privacy-first web search.
```json
{"q": "ai agents 2026"}
```
Optional: `count`, `country`, `search_lang`, `freshness`

### news-search ($0.04) — powered by Brave
Search recent news articles across the web.
```json
{"q": "openai funding round"}
```

### image-search ($0.04) — powered by Brave
Search for images across the web.
```json
{"q": "golden gate bridge"}
```

### web-answer ($0.01) — powered by Exa
Get an AI-powered answer with cited sources.
```json
{"query": "What is the latest valuation of SpaceX?"}
```

Browse all search capabilities: `clawcard agent catalog-search "search" --json`
