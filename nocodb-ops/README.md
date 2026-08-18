# nocodb-ops (Codex plugin)

NocoDB ops plugin for Agents Store. Record management, views, reports, filtering, search, and data import/export for business users via MCP tools and CLI.

## Install

```bash
codex plugin marketplace add agents-store/nocodb-ops-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 15 skill(s) under `skills/` (includes 6 command(s) converted to skills — Codex has no custom slash-command system)
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- MCP server config pointed to from the manifest (`.mcp.json`) — see AGENTS.md for the `~/.codex/config.toml` snippet
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/nocodb-ops
