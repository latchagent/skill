# Catalog: Research

## How to call

```
clawcard agent wallet send --url "https://clawcard.sh/api/catalog/<capability>" --method POST --body '<json>' --json
```

## Capabilities (all live)

### web-search ($0.005) — powered by Exa
Search the web with neural, auto, or deep search modes. Returns results with highlights and content.
```json
{"query": "Latest developments in LLM capabilities"}
```
Additional params: `type` (neural/auto/deep/instant), `numResults`, `category` (company/research paper/news/people), `includeDomains`, `excludeDomains`, `startPublishedDate`, `endPublishedDate`

### scrape-url ($0.02) — powered by Exa
Extract full text, highlights, and summaries from any URL.
```json
{"url": "https://arxiv.org/abs/2307.06435"}
```
Pass multiple URLs: `{"urls": ["url1", "url2"]}`. Additional params: `text` (true or {maxCharacters}), `highlights`, `summary`

### deep-research ($0.05) — powered by Exa
Get an LLM-generated answer with cited sources.
```json
{"topic": "What is the latest valuation of SpaceX?"}
```
Returns `{answer, citations}`. Supports `outputSchema` for structured JSON answers.

### research-task ($0.10, async) — powered by Exa
Launch a deep async research task that explores the web, gathers sources, and returns a detailed report with citations.
```json
{"instructions": "Summarize the latest developments in AI safety research"}
```
Models: `exa-research-fast`, `exa-research` (default), `exa-research-pro`

**This is async.** The response returns a `researchId` and `pollUrl`:
```json
{"researchId": "01jsz...", "status": "pending", "pollUrl": "/api/catalog/research-task/status?researchId=01jsz..."}
```

Poll for results (no additional payment needed):
```
curl https://clawcard.sh/api/catalog/research-task/status?researchId=01jsz...
```

When `status` is `completed`, the `output.content` field contains the full research report. Supports `outputSchema` for structured JSON output.
