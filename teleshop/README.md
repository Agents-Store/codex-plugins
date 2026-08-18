# teleshop (Codex plugin)

Teleshop store management plugin. Manage products, orders, categories, attributes, customers, webhooks, and addons for your Telegram store via 50 MCP tools.

## Install

```bash
codex plugin marketplace add agents-store/teleshop-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 22 skill(s) under `skills/` (includes 13 command(s) converted to skills — Codex has no custom slash-command system)
- 2 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- MCP server config pointed to from the manifest (`.mcp.json`) — see AGENTS.md for the `~/.codex/config.toml` snippet
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/teleshop
