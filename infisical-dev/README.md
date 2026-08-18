# infisical-dev (Codex plugin)

Infisical CLI dev plugin for Agents Store. Complete command-line coverage for secrets management — install & auth, infisical run/secrets/export, dynamic secrets, secret scanning with pre-commit hooks, machine-identity CI/CD auth, self-hosted, and troubleshooting.

## Install

```bash
codex plugin marketplace add agents-store/infisical-dev-codex
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

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/infisical-dev
