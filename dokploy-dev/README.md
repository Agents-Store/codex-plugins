# dokploy-dev (Codex plugin)

Dokploy self-hosted PaaS development plugin. Deploy applications, provision databases (Postgres, MySQL, MariaDB, MongoDB, Redis, LibSQL), manage domains, Docker Compose stacks, backups, and server operations via the official @dokploy/mcp server (500+ tools across 49 categories), 463 REST API endpoints, and CLI.

## Install

```bash
codex plugin marketplace add agents-store/dokploy-dev-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 6 skill(s) under `skills/`
- 1 subagent(s) under `.codex/agents/`
- MCP server config in `.mcp.json`
- 8 workflow template(s) — see AGENTS.md

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/dokploy-dev
