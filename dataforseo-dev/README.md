# dataforseo-dev (Codex plugin)

DataForSEO data analysis plugin. Keyword research, competitor analysis, backlink auditing, SERP monitoring, on-page audits, content analysis, and AI optimization via 70+ MCP tools.

## Install

```bash
codex plugin marketplace add agents-store/dataforseo-dev-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 13 skill(s) under `skills/` (includes 3 command(s) converted to skills — Codex has no custom slash-command system)
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- MCP server config pointed to from the manifest (`.mcp.json`) — see AGENTS.md for the `~/.codex/config.toml` snippet
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/dataforseo-dev
