# flask-dev (Codex plugin)

Flask dev plugin for Agents Store. Application factory patterns, blueprint organization, Jinja2 templates, Flask CLI recipes, and troubleshooting for developers building with Flask.

## Install

```bash
codex plugin marketplace add agents-store/flask-dev-codex
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

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/flask-dev
