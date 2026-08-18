# nextjs-dev (Codex plugin)

Next.js development plugin. Knowledge base for building modern Next.js 16 applications with App Router, Server/Client Components, data fetching, Cache Components, performance optimization, and the next-devtools-mcp toolchain.

## Install

```bash
codex plugin marketplace add agents-store/nextjs-dev-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 18 skill(s) under `skills/`
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- No MCP server
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/nextjs-dev
