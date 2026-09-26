# nocodb (Codex plugin)

DEPRECATED — superseded by nocodb-dev (schema, fields, views, Meta API) and nocodb-ops (records, search, views, reports), which teach the current NocoDB MCP tool names; this plugin teaches tool names the server no longer exposes. NocoDB database development plugin. Manage tables, records, columns, views, relations, formulas, rollups, lookups, filtering, sorting, search, aggregation, webhooks, and filter/sort management via MCP tools.

## Install

```bash
codex plugin marketplace add agents-store/nocodb-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 18 skill(s) under `skills/` (includes 10 command(s) converted to skills — Codex has no custom slash-command system)
- 2 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- MCP server config pointed to from the manifest (`.mcp.json`) — see AGENTS.md for the `~/.codex/config.toml` snippet
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/nocodb
