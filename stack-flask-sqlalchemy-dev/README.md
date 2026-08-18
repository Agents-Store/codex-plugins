# stack-flask-sqlalchemy-dev (Codex plugin)

Flask + SQLAlchemy stack dev plugin for Agents Store. Integration patterns for app factory wiring, blueprint-model coordination, Flask-Login + SQLAlchemy auth, Jinja2 + query data, and full-feature recipes.

## Install

```bash
codex plugin marketplace add agents-store/stack-flask-sqlalchemy-dev-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 4 skill(s) under `skills/`
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- No MCP server
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/stack-flask-sqlalchemy-dev
