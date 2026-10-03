# stack-directus-nextjs-trigger (Codex plugin)

Directus + Next.js + Trigger.dev architecture plugin. How Directus (content, files, access), a Next.js App Router frontend and self-hosted Trigger.dev (durable and scheduled work) fit together: who holds which token, how the cache is invalidated, how a Directus change reaches a task and the result reaches the page, who owns the session, and a production checklist. Tool knowledge comes from its dependencies.

## Install

```bash
codex plugin marketplace add agents-store/stack-directus-nextjs-trigger-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 8 skill(s) under `skills/`
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- MCP server config pointed to from the manifest (`.mcp.json`) — see AGENTS.md for the `~/.codex/config.toml` snippet
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/stack-directus-nextjs-trigger
