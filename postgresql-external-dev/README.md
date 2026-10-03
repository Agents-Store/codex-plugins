# postgresql-external-dev (Codex plugin)

PostgreSQL knowledge for low-code stacks. Schema design for external database connections (compatible SQL patterns for NocoDB and NocoBase — table creation, column types, relations, indexes, anti-patterns), plus the 29-tool PostgreSQL MCP reference and the PostgREST REST API.

## Install

```bash
codex plugin marketplace add agents-store/postgresql-external-dev-codex
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

- No MCP server
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/postgresql-external-dev
