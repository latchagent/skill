# Catalog: Research, GTM & Lead Gen

Capabilities for web research, competitive intelligence, and lead generation. Currently stubbed — coming soon.

## How to call

```
clawcard agent wallet send --url "https://clawcard.sh/api/catalog/<capability>" --method POST --body '<json>' --json
```

## Research (coming soon)

- `web-search` ($0.005) — Search the web. Body: `{"query": "..."}`
- `scrape-url` ($0.02) — Extract content from a URL. Body: `{"url": "..."}`
- `deep-research` ($0.05) — In-depth research on any topic. Body: `{"topic": "..."}`

## GTM (coming soon)

- `keyword-research` ($0.01) — Keyword opportunities with volume data. Body: `{"keyword": "..."}`
- `competitor-analysis` ($0.02) — Competitor website and strategy analysis. Body: `{"domain": "..."}`

## Lead Gen (coming soon)

- `find-contacts` ($0.03) — Find contacts at target companies. Body: `{"company": "..."}`
- `enrich-company` ($0.02) — Company data from a domain or name. Body: `{"domain": "..."}`
- `enrich-prospect` ($0.02) — Prospect enrichment with social profiles. Body: `{"email": "..."}`
