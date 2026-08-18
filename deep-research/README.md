# deep-research (Codex plugin)

Deep Research plugin. Comprehensive web research using 4 providers (Exa, Firecrawl, Jina, Perplexity) with capability-based CONNECTORS pattern and automatic FALLBACK chains. Search, scrape, crawl, extract — each action tries multiple providers until one succeeds.

## Install

```bash
codex plugin marketplace add agents-store/deep-research-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 11 skill(s) under `skills/` (includes 6 command(s) converted to skills — Codex has no custom slash-command system)
- MCP server config pointed to from the manifest (`.mcp.json`) — see AGENTS.md for the `~/.codex/config.toml` snippet
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/deep-research
