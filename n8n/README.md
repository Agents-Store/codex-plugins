# n8n (Codex plugin)

DEPRECATED — superseded by n8n-dev (building, validating and debugging workflows against both current n8n MCP servers) and n8n-provision (template discovery and batch import). Use those two instead. n8n workflow automation plugin. Manage workflows, execute automations, configure nodes, handle credentials, monitor executions, expression syntax, node configuration patterns, and code node best practices via MCP tools.

## Install

```bash
codex plugin marketplace add agents-store/n8n-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 17 skill(s) under `skills/` (includes 9 command(s) converted to skills — Codex has no custom slash-command system)
- 2 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- MCP server config pointed to from the manifest (`.mcp.json`) — see AGENTS.md for the `~/.codex/config.toml` snippet
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/n8n
