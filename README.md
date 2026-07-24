# PostLake — MCP server & agent skills for social media

**PostLake is the social media API for AI agents.** One integration to publish,
schedule, and measure across every network — X, LinkedIn, Instagram, TikTok,
Facebook, Threads, Bluesky, YouTube and Pinterest — with one normalised response
shape an agent can reason over instead of nine different error formats.

This repo holds the **agent skills** and documents the **hosted MCP server**.

- 🔌 **MCP server:** `https://api.postlake.dev/mcp` (streamable-http, OAuth)
- 📚 **Docs:** https://postlake.dev/docs/
- 🌐 **Site:** https://postlake.dev
- 📖 **For LLMs:** https://postlake.dev/llms.txt

---

## Quick start — MCP

PostLake runs a **hosted** MCP server. There's nothing to install or self-host:
point your client at the URL and approve once over OAuth. Your agent never sees
an API key.

### Claude

```bash
claude mcp add postlake https://api.postlake.dev/mcp
```

Or in Claude Desktop: **Settings → Connectors → Add custom connector**.

### Cursor

`~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "postlake": { "url": "https://api.postlake.dev/mcp" }
  }
}
```

### Gemini CLI

```bash
gemini mcp add postlake https://api.postlake.dev/mcp
```

### VS Code / GitHub Copilot

**Agent mode → MCP servers → Add**, then paste the same URL.

### Header auth (for clients without OAuth)

```
POST https://api.postlake.dev/v1/mcp
Authorization: Bearer <your_api_key>
```

Then just ask:

> *"Post this photo to Instagram and LinkedIn, schedule a follow-up for Friday
> at 9am, and tell me which of last week's posts did best."*

---

## The toolset

A focused set — everything an agent needs to publish and reason about the result,
and nothing it doesn't. Account setup and connecting networks stay in the
dashboard, where a human belongs.

| Group | Tools |
|---|---|
| **Publish & schedule** | `create_post` · `edit_post` · `cancel_post` · `get_post` · `list_posts` |
| **Discover & validate** | `get_platform_capabilities` · `validate_post` · `get_publish_info` |
| **Channels & media** | `list_social_accounts` · `list_account_targets` · `list_profiles` · `upload_media` |
| **Analytics** | `get_analytics` · `get_post_analytics` |
| **Identity** | `whoami` |

**Agent-safe by design.** Publishing is idempotent — pass an `Idempotency-Key`
and a retrying or restarting agent can never double-post. Every post returns a
per-platform `state` and `url` in one shape, so there's no per-network branching.

---

## Agent skills

For coding agents (Claude Code, Cursor, Codex, Windsurf) that use file-based
skills rather than MCP:

```bash
npx skills add crumblelake/postlake-skills --all
```

| Skill | What your agent can do |
|-------|------------------------|
| `postlake-accounts`  | List connected accounts; see what platforms are ready |
| `postlake-publish`   | Publish a post now to one or more platforms |
| `postlake-schedule`  | Schedule for later; list, reschedule, or cancel |
| `postlake-media`     | Upload an image or video and attach it to a post |
| `postlake-analytics` | Read per-post and cross-platform performance |

Set your key once:

```bash
export POSTLAKE_API_KEY="sk_live_…"
```

Create one at https://app.postlake.dev/app/keys.

---

## Three ways to connect

| | Best for | Auth |
|---|---|---|
| **MCP server** | Claude, Cursor, ChatGPT, Gemini, any MCP client | OAuth (no key exposed) |
| **Agent skills** (this repo) | Claude Code, Cursor, Codex, Windsurf | `POSTLAKE_API_KEY` |
| **REST API** | LangChain, CrewAI, AutoGen, OpenAI tools, any language | Bearer key |

REST base URL: `https://api.postlake.dev/v1` — see the
[full docs](https://postlake.dev/docs/) and per-framework guides for
[LangChain](https://postlake.dev/guides/langchain),
[CrewAI](https://postlake.dev/guides/crewai),
[AutoGen](https://postlake.dev/guides/autogen),
[Python](https://postlake.dev/guides/python) and
[Node.js](https://postlake.dev/guides/nodejs).

---

## Security

Agents authorise over OAuth, so an API key is never pasted into a chat or seen by
the model. Every agent you authorise appears under **Connected Apps** in your
dashboard — revoke any one and its access is cut off immediately.

## Pricing

Usage-based credits: **1 credit per post**, with a free tier and no card to start.
X costs more because it's the only network that charges per post.
See https://postlake.dev/pricing.

---

Listed in the [official MCP Registry](https://registry.modelcontextprotocol.io)
as `dev.postlake/social`.

Made by [CrumbleLake](https://postlake.dev). Issues and PRs welcome.
