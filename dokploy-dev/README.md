# dokploy-dev (Codex plugin)

Dokploy self-hosted PaaS development plugin (aligned with Dokploy v0.29.5). Deploy applications, provision 6 database types (Postgres, MySQL, MariaDB, MongoDB, Redis, LibSQL), manage domains and Docker Compose stacks, AND debug failed deployments end-to-end — reads runtime logs of every container (including each container in a Docker Compose stack) over the API/MCP with tail/since/search, plus AI-powered log analysis (ai-analyzeLogs), Docker container introspection, Traefik diagnosis, and a guided recovery chain. Complete MCP/REST coverage: all 526 v0.29.5 operations across 49 categories indexed with params. Uses the official @dokploy/mcp server plus debugging-focused slash commands including /compose-logs.

## Install

```bash
codex plugin marketplace add agents-store/dokploy-dev-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 9 skill(s) under `skills/`
- 1 subagent(s) under `.codex/agents/`
- MCP server config in `.mcp.json`
- 14 workflow template(s) — see AGENTS.md

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/dokploy-dev
