# web-search-dev (Codex plugin)

Web search and scraping developer toolkit. MCP tool patterns, REST API reference (Firecrawl v2), SDK/CLI usage for Firecrawl, Exa, Perplexity, Jina, Pexels, Unsplash, and Context7. Practical skills for web scraping, documentation search, and media discovery in dev workflows.

## Install

```bash
codex plugin marketplace add agents-store/web-search-dev-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 10 skill(s) under `skills/`
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- MCP server config pointed to from the manifest (`.mcp.json`) — see AGENTS.md for the `~/.codex/config.toml` snippet
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/web-search-dev
