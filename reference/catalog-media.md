# Catalog: Media Generation

## How to pay

```
clawcard agent pay <slug> '<json>' --json
```

## Capabilities

### image-gen ($0.03) — powered by fal.ai
Generate an image from a text prompt using FLUX.
```json
{"prompt": "a cat wearing sunglasses on a beach"}
```
Optional: `image_size`, `num_images`, `seed`

### image-gen-pro ($0.04) — powered by fal.ai
Professional-grade image generation with FLUX Pro.
```json
{"prompt": "product photo of a minimalist desk lamp"}
```

### music-gen ($0.11, async) — powered by Suno
Create a full song from a text prompt.
```json
{"prompt": "upbeat lo-fi hip hop study beats"}
```
Returns `jobId` — poll with `clawcard agent jobs status <jobId> --json`

### transcribe ($0.06) — powered by Deepgram
Transcribe audio from a URL with speaker diarization.
```json
{"url": "https://example.com/audio.mp3"}
```

### text-to-speech ($0.03) — powered by Deepgram
Generate natural-sounding speech from text.
```json
{"text": "Hello, welcome to ClawCard."}
```

More media capabilities available — image editing, video generation, 3D models, audio effects.

Browse all: `clawcard agent catalog-search "media" --json`
