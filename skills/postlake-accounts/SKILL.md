---
name: postlake-accounts
description: List the social accounts connected to PostLake (X, LinkedIn, Instagram, TikTok, Facebook, Threads, Bluesky, YouTube, Pinterest) and their status. Use this first, before publishing, to get the account ids to post to.
---

# PostLake — connected accounts

Use this before publishing: it returns the `acc_…` ids you pass as `accounts`
when creating a post, plus each account's platform, handle, and health status.

## Auth

All requests use a Bearer key from the `POSTLAKE_API_KEY` environment variable:

```
Authorization: Bearer $POSTLAKE_API_KEY
```

Get a key at https://app.postlake.dev/app/keys. Base URL: `https://api.postlake.dev`.

## List connected accounts

```bash
curl https://api.postlake.dev/v1/social-accounts \
  -H "Authorization: Bearer $POSTLAKE_API_KEY"
```

Response:

```json
{
  "accounts": [
    { "id": "acc_84a4…", "platform": "bluesky",  "handle": "yourbrand.bsky.social", "status": "active" },
    { "id": "acc_e553…", "platform": "linkedin", "handle": "Your Name",             "status": "active" },
    { "id": "acc_54d8…", "platform": "instagram","handle": "yourbrand",             "status": "needs_reauth" }
  ]
}
```

- `status: "active"` — ready to post.
- `status: "needs_reauth"` — the connection expired; the user must reconnect it at
  https://app.postlake.dev/app/channels before posting to that account.

## Connecting new accounts

Connecting a platform is an OAuth flow a human completes in the browser. If a
platform the user wants isn't listed, tell them to connect it at
**https://app.postlake.dev/app/channels**, then re-list.

## What platforms support

To check limits/options for a platform (character limits, media rules) before
composing, call the public capabilities endpoint (no auth needed):

```bash
curl https://api.postlake.dev/v1/platforms/bluesky
```

## Tips

- Always resolve account ids from here rather than guessing.
- Skip accounts with `status: "needs_reauth"` and tell the user to reconnect.
