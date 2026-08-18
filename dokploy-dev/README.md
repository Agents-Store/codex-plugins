# dokploy-dev (Codex plugin)

Dokploy self-hosted PaaS development plugin (aligned with Dokploy v0.29.14). Deploy applications, provision 6 database types (Postgres, MySQL, MariaDB, MongoDB, Redis, LibSQL), manage domains and Docker Compose stacks, AND debug failed deployments end-to-end — reads runtime logs of every container (including each container in a Docker Compose stack) over the API/MCP with tail/since/search, plus AI-powered log analysis (ai-analyzeLogs), Docker container introspection, Traefik diagnosis, and a guided recovery chain. Complete MCP/REST coverage: all 546 v0.29.14 operations across 50 categories indexed with params — covers forward-auth SSO domain protection, SCIM provisioning, build concurrency, and the rewritten @dokploy/cli (546 auto-generated commands incl. read-logs). Uses the official @dokploy/mcp server plus debugging-focused slash commands including /compose-logs.

## Install

```bash
codex plugin marketplace add agents-store/dokploy-dev-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 23 skill(s) under `skills/` (includes 14 command(s) converted to skills — Codex has no custom slash-command system)
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- MCP server config pointed to from the manifest (`.mcp.json`) — see AGENTS.md for the `~/.codex/config.toml` snippet
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/dokploy-dev
