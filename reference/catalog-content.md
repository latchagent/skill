# Catalog: Content & Async Capabilities

Content generation capabilities. Currently stubbed — coming soon.

## How to call

```
clawcard agent wallet send --url "https://clawcard.sh/api/catalog/<capability>" --method POST --body '<json>' --json
```

## Content (coming soon)

- `generate-image` ($0.05, async) — Generate images from prompts. Body: `{"prompt": "..."}`
- `generate-ugc` ($0.10, async) — Generate UGC-style videos. Body: `{"prompt": "..."}`
- `seo-content` ($0.05) — SEO-optimized content for keywords. Body: `{"keyword": "..."}`

## Async Capabilities

Some capabilities (generate-image, generate-ugc) are async. They return a `jobId`:

```json
{"jobId": "job_abc123", "status": "pending"}
```

Poll for completion:
```
clawcard agent jobs status <job-id> --json
```

When complete, download the artifact:
```
clawcard agent artifacts download <art-id> --json
```

## Artifacts & Jobs

List artifacts:
```
clawcard agent artifacts list --json
```

List jobs:
```
clawcard agent jobs list --json
```
