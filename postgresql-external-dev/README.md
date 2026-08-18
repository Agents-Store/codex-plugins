# postgresql-external-dev (Codex plugin)

PostgreSQL schema design for external database connections. Compatible SQL patterns for NocoDB and NocoBase — table creation, column types, relations, indexes, and anti-patterns.

## Install

```bash
codex plugin marketplace add agents-store/postgresql-external-dev-codex
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

- No MCP server
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/postgresql-external-dev
