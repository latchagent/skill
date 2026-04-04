# Catalog: Compute

## How to pay

```
clawcard agent pay <slug> '<json>' --json
```

## Capabilities

### run-code ($0.01) — powered by Judge0
Run source code in 60+ languages with sandboxed isolation.
```json
{"source_code": "print('hello world')", "language_id": 71}
```
Optional: `stdin`, `cpu_time_limit`, `memory_limit`

### screenshot ($0.06) — powered by ScreenshotOne
Capture a screenshot of any URL as PNG, JPEG, or PDF.
```json
{"url": "https://clawcard.sh", "full_page": true}
```
Optional: `format`, `viewport_width`, `viewport_height`, `delay`
