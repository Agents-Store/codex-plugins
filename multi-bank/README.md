# multi-bank (Codex plugin)

Multi-Bank Account Manager with broadcast architecture pattern. Aggregates financial data from Monobank and PrivatBank via MCP tools, broadcasts balance updates and budget alerts to subscribed components, categorizes transactions, and exports financial reports in CSV/PDF.

## Install

```bash
codex plugin marketplace add agents-store/multi-bank-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 15 skill(s) under `skills/`
- 2 subagent(s) under `.codex/agents/`
- MCP server config in `.mcp.json`
- 14 workflow template(s) — see AGENTS.md

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/multi-bank
