# grammy-dev (Codex plugin)

grammY (Telegram bot framework) dev plugin for Agents Store. Covers bot core, filter queries, middleware, commands, keyboards, sessions, conversations, files, payments, deployment, scaling, and the full @grammyjs/* plugin catalog for Node.js/Deno/TypeScript bot developers.

## Install

```bash
codex plugin marketplace add agents-store/grammy-dev-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 16 skill(s) under `skills/`
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- No MCP server
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/grammy-dev
