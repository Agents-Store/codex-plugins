# stack-ads (Codex plugin)

Paid advertising stack for any project. Researches the product and its market, derives targeting, writes ad copy, generates images, video and voice-over, launches paused campaigns on Google Ads, Meta and TikTok from a reviewed dry-run plan with budget ceilings and a rollback ledger, and reports results. Walks the user through obtaining every key. Integrates Google Ads, Search Console, Meta, TikTok, Firecrawl, Apify, DataForSEO, OpenAI, Gemini and fal.ai via MCP and one dependency-free toolkit.

## Install

```bash
codex plugin marketplace add agents-store/stack-ads-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 11 skill(s) under `skills/`
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- MCP server config pointed to from the manifest (`.mcp.json`) — see AGENTS.md for the `~/.codex/config.toml` snippet
- Hooks in `hooks/hooks.json` — run `/hooks` after install to trust them

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/stack-ads
