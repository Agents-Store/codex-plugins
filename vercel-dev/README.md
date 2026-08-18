# vercel-dev (Codex plugin)

Vercel ecosystem plugin. Deployment, AI SDK, Edge Functions, storage, routing, performance optimization. Includes CLI deploy troubleshooting for non-Git projects, Hobby plan fixes, standalone output handling. Based on official vercel-plugin v0.25.0 by Vercel Labs.

## Install

```bash
codex plugin marketplace add agents-store/vercel-dev-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 31 skill(s) under `skills/` (includes 6 command(s) converted to skills — Codex has no custom slash-command system)
- 3 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- MCP server config pointed to from the manifest (`.mcp.json`) — see AGENTS.md for the `~/.codex/config.toml` snippet
- Hooks in `hooks/hooks.json` — run `/hooks` after install to trust them

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/vercel-dev
