# stack-directus-nextjs-dev (Codex plugin)

Directus + Next.js stack dev plugin. Integrates Directus headless CMS with Next.js App Router for content-driven applications.

## Install

```bash
codex plugin marketplace add agents-store/stack-directus-nextjs-dev-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 6 skill(s) under `skills/`
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- MCP server config pointed to from the manifest (`.mcp.json`) — see AGENTS.md for the `~/.codex/config.toml` snippet
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/stack-directus-nextjs-dev
