# nocodb-dev (Codex plugin)

NocoDB schema development plugin. Full Meta API v3 coverage — tables, fields (30+ types), views, filters, sorts, hooks (HookV3), comments, scripts, dashboards & widgets, workflows, plus workspaces / members / teams / tokens. Bundles both Data API and Meta API OpenAPI specs.

## Install

```bash
codex plugin marketplace add agents-store/nocodb-dev-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 18 skill(s) under `skills/` (includes 6 command(s) converted to skills — Codex has no custom slash-command system)
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- MCP server config pointed to from the manifest (`.mcp.json`) — see AGENTS.md for the `~/.codex/config.toml` snippet
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/nocodb-dev
