# trigger-dev (Codex plugin)

Trigger.dev dev plugin for Agents Store. Comprehensive development knowledge for building background tasks, AI agent workflows, and durable execution on self-hosted Trigger.dev v4.4.4 — SDK patterns, CLI recipes, deployment, full 33-tool MCP reference, TRQL queries and dashboards, managed prompts, dev-server control, realtime API, and troubleshooting.

## Install

```bash
codex plugin marketplace add agents-store/trigger-dev-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 16 skill(s) under `skills/` (includes 4 command(s) converted to skills — Codex has no custom slash-command system)
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- No MCP server
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/trigger-dev
