# sqlalchemy-dev (Codex plugin)

SQLAlchemy dev plugin for Agents Store. Model definition patterns, relationship mapping, query optimization, Alembic migrations, and troubleshooting for developers building with SQLAlchemy 2.0+.

## Install

```bash
codex plugin marketplace add agents-store/sqlalchemy-dev-codex
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

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/sqlalchemy-dev
