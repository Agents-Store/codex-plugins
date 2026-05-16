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

- 5 skill(s) under `skills/`
- 0 subagent(s) under `.codex/agents/`
- MCP server config in `.mcp.json`
- 6 workflow template(s) — see AGENTS.md

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/deep-research
