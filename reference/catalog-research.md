# Catalog: Research, GTM & Lead Gen

## How to call

```
clawcard agent wallet send --url "https://clawcard.sh/api/catalog/<capability>" --method POST --body '<json>' --json
```

## Research (live)

### web-search ($0.005)
Search the web with neural, auto, or deep search modes. Returns results with highlights and content.
```json
{"query": "Latest developments in LLM capabilities"}
```
Additional params: `type` (neural/auto/deep/instant), `numResults`, `category` (company/research paper/news/people), `includeDomains`, `excludeDomains`, `startPublishedDate`, `endPublishedDate`

### scrape-url ($0.02)
Extract full text, highlights, and summaries from any URL.
```json
{"url": "https://arxiv.org/abs/2307.06435"}
```
Pass multiple URLs: `{"urls": ["url1", "url2"]}`. Additional params: `text` (true or {maxCharacters}), `highlights`, `summary`

### deep-research ($0.05)
Get an LLM-generated answer with cited sources.
```json
{"topic": "What is the latest valuation of SpaceX?"}
```
Returns `{answer, citations}`. Supports `outputSchema` for structured JSON answers.

## GTM (coming soon)

- `keyword-research` ($0.01) — Keyword opportunities with volume data. Body: `{"keyword": "..."}`
- `competitor-analysis` ($0.02) — Competitor website and strategy analysis. Body: `{"domain": "..."}`

## Lead Gen (coming soon)

- `find-contacts` ($0.03) — Find contacts at target companies. Body: `{"company": "..."}`
- `enrich-company` ($0.02) — Company data from a domain or name. Body: `{"domain": "..."}`
- `enrich-prospect` ($0.02) — Prospect enrichment with social profiles. Body: `{"email": "..."}`
