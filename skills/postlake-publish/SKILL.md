---
name: postlake-publish
description: Publish a social media post right now to one or more connected accounts (X, LinkedIn, Instagram, TikTok, Facebook, Threads, Bluesky, YouTube, Pinterest) through PostLake. Use when the user wants to post immediately.
---

# PostLake — publish a post now

Publishes one caption to as many connected accounts as you like, in one call.
Each platform is attempted independently and the response reports per-platform
`state` and `url`.

## Auth

```
Authorization: Bearer $POSTLAKE_API_KEY
```

Base URL `https://api.postlake.dev`. Get the account ids to post to from the
`postlake-accounts` skill (`GET /v1/social-accounts`).

## Publish

```bash
curl -X POST https://api.postlake.dev/v1/posts \
  -H "Authorization: Bearer $POSTLAKE_API_KEY" \
  -H "Content-Type: application/json" \
  -H "Idempotency-Key: $(uuidgen)" \
  -d '{
    "text": "Shipping something new today 🚀",
    "accounts": ["acc_84a4…", "acc_e553…"]
  }'
```

Response (normalised — reason over `targets`):

```json
{
  "id": "post_…",
  "state": "published",
  "targets": [
    { "platform": "bluesky",  "state": "published", "url": "https://bsky.app/profile/…/post/…" },
    { "platform": "linkedin", "state": "published", "url": "https://www.linkedin.com/feed/update/…" }
  ]
}
```

`state` per target is one of `published`, `processing` (async platforms like
TikTok/YouTube — poll `GET /v1/posts/{id}` until final), `failed` (see
`target.error`), or `scheduled`.

## Options

- **Attach media**: upload first with the `postlake-media` skill, then pass the
  ids: `"media": ["med_…"]` (with optional `"mediaAlt": ["alt text"]`).
- **Per-platform captions**: `"textOverrides": { "x": "shorter for X", "linkedin": "…" }`.
- **Platform-specific settings**: `"platformOptions": { "pinterest": { "boardId": "…" }, "youtube": { "title": "…", "privacyStatus": "private" } }`.

## Rules to respect

- **Idempotency**: always send a unique `Idempotency-Key` header so a retry replays
  the same result instead of double-posting.
- **X**: needs a paid plan (X is the only network that charges). On a free plan an
  X target returns `error.type: "entitlement_exceeded"` — other platforms still go
  out. Don't treat that as a total failure.
- **Media-required platforms**: Instagram, TikTok, YouTube, Pinterest need media —
  a text-only post to them fails with `invalid_input`.
- A `failed` target never sinks the others; report which went out and which didn't.
