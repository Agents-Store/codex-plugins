# stack-directus-nextjs-trigger-dev (Codex plugin)

Directus + Next.js + Trigger.dev stack dev plugin. Adds self-hosted Trigger.dev as a workflow engine for AI agents, durable async logic, and scheduled jobs on top of the Directus + Next.js App Router stack.

## Install

```bash
codex plugin marketplace add agents-store/stack-directus-nextjs-trigger-dev-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 9 skill(s) under `skills/`
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- MCP server config pointed to from the manifest (`.mcp.json`) — see AGENTS.md for the `~/.codex/config.toml` snippet
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/stack-directus-nextjs-trigger-dev
