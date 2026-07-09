# PostLake agent skills

**PostLake is the social media API for AI agents** — one integration to publish,
schedule, and measure across every network (X, LinkedIn, Instagram, TikTok,
Facebook, Threads, Bluesky, YouTube, Pinterest).

These are drop-in **agent skills**: install them and your coding agent (Claude
Code, Cursor, Codex, Windsurf, …) already knows how to run your socials. Just ask.

## Install

```bash
npx skills add crumblelake/postlake-skills --all
```

That adds five skills:

| Skill | What your agent can do |
|-------|------------------------|
| `postlake-accounts`  | List connected accounts; see what platforms are ready |
| `postlake-publish`   | Publish a post now to one or more platforms |
| `postlake-schedule`  | Schedule for later; list, reschedule, or cancel |
| `postlake-media`     | Upload an image or video and attach it to a post |
| `postlake-analytics` | Read per-post and cross-platform performance |

## Setup (once)

1. Create a PostLake account and an API key at **https://app.postlake.dev/app/keys**.
2. Make the key available to your agent as an environment variable:

   ```bash
   export POSTLAKE_API_KEY="sk_live_…"
   ```

3. Ask your agent, e.g. *"post this to LinkedIn and Bluesky, and schedule a recap
   for Friday 9am."*

## Three ways to connect

- **Skill** (these files) — for Claude Code, Cursor, Codex, Windsurf.
- **MCP server** — `https://api.postlake.dev/mcp` (OAuth) for Claude Desktop, Zed,
  and other MCP clients; or `https://api.postlake.dev/v1/mcp` with a Bearer key.
- **REST API** — `https://api.postlake.dev/v1` for any framework (LangChain,
  CrewAI, OpenAI tools). Full docs: https://postlake.dev/docs

## Notes

- Every post returns one normalised shape with a per-platform `state` and `url`,
  so an agent can reason over the result.
- Publishing is idempotent — pass an `Idempotency-Key` header so a retry never
  double-posts.
- Billing is usage-based credits (1 credit/post). X costs more and needs a paid
  plan (X is the only network that charges us).

Made by [CrumbleLake](https://postlake.dev). Issues and PRs welcome.
