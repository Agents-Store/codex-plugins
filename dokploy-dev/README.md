# dokploy-dev (Codex plugin)

Dokploy self-hosted PaaS development plugin (aligned with Dokploy v0.29.x). Deploy applications, provision 6 database types (Postgres, MySQL, MariaDB, MongoDB, Redis, LibSQL), manage domains and Docker Compose stacks, AND debug failed deployments end-to-end with AI-powered log analysis (ai-analyzeLogs), Docker container introspection, Traefik diagnosis, and a guided recovery chain. Uses the official @dokploy/mcp server (500+ tools across 49 categories) plus 5 debugging-focused slash commands.

## Install

```bash
codex plugin marketplace add agents-store/dokploy-dev-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 8 skill(s) under `skills/`
- 1 subagent(s) under `.codex/agents/`
- MCP server config in `.mcp.json`
- 13 workflow template(s) — see AGENTS.md

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/dokploy-dev
