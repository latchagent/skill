# Catalog: Social Data

Scrape public social media data across 15+ platforms. All calls use x402 payment via your wallet.

## How to call

```
clawcard agent wallet send --url "https://clawcard.sh/api/catalog/<capability>" --method POST --body '<json>' --json
```

## Capabilities

### social-profile ($0.02)
Get a creator's public profile — followers, bio, avatar, stats.
```json
{"platform": "tiktok", "handle": "stoolpresidente"}
```

### social-posts ($0.03)
Get recent posts, videos, or reels with engagement metrics.
```json
{"platform": "instagram", "handle": "jane"}
```

### social-search ($0.02)
Search posts, videos, users, hashtags, ads, and shop products.
```json
{"platform": "youtube", "query": "ai agents"}
```

### social-post ($0.02)
Get full details on a specific post/video — engagement, media URLs, captions.
```json
{"url": "https://www.tiktok.com/@user/video/123"}
```
Platform is auto-detected from URL. Override with `"platform"` if needed.

### social-comments ($0.02)
Get comments and replies with pagination.
```json
{"url": "https://www.youtube.com/watch?v=dQw4w9WgXcQ"}
```

### social-transcript ($0.03)
Extract spoken transcript from a video. Great for content analysis.
```json
{"url": "https://www.youtube.com/watch?v=abc123"}
```

## Supported Platforms

| Platform | profile | posts | search | post | comments | transcript |
|---|---|---|---|---|---|---|
| TikTok | yes | yes | yes | yes | yes | yes |
| Instagram | yes | yes | yes | yes | yes | yes |
| YouTube | yes | yes | yes | yes | yes | yes |
| Twitter/X | yes | yes | — | yes | — | yes |
| LinkedIn | yes | yes | — | yes | — | — |
| Facebook | yes | yes | — | yes | yes | yes |
| Reddit | yes | yes | yes | yes | yes | — |
| Threads | yes | yes | yes | yes | — | — |
| Bluesky | yes | yes | — | yes | — | — |
| Snapchat | yes | — | — | — | — | — |
| Truth Social | yes | yes | — | — | — | — |
| Twitch | yes | — | — | — | — | — |
| Pinterest | — | — | yes | yes | — | — |
| Google | — | — | yes | — | — | — |

## Search Variants

Some platforms have specialized search sub-types:
- `tiktok-users` — search TikTok users
- `tiktok-hashtag` — search by hashtag
- `tiktok-shop` — search TikTok Shop products
- `facebook-ads` — search Facebook Ad Library
- `google-ads` — search Google Ad Library
- `linkedin-ads` — search LinkedIn Ad Library
- `reddit-ads` — search Reddit Ad Library

Pass these as the `platform` value in social-search.

## Additional Parameters

Each endpoint accepts platform-specific parameters in the body alongside `platform`:
- `cursor` / `max_cursor` / `continuationToken` — pagination
- `sort` / `sort_by` — sorting (latest, popular, relevance)
- `trim` — set to `"true"` for trimmed responses
- `region` — proxy region (2-letter country code)
- `limit` / `page` — result limits

Run `clawcard agent catalog --json` for full details.
