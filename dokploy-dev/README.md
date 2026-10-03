# dokploy-dev (Codex plugin)

Dokploy self-hosted PaaS development plugin (aligned with Dokploy v0.30.x). Deploy applications, provision 6 database types (Postgres, MySQL, MariaDB, MongoDB, Redis, LibSQL), manage domains and Docker Compose stacks, AND debug failed deployments end-to-end — reads runtime logs of every container (including each container in a Docker Compose stack) over the API/MCP with tail/since/search, plus AI-powered log analysis (ai-analyzeLogs), Docker container and host introspection (server health, events, disk usage, images, volumes), Traefik diagnosis, and a guided recovery chain. Complete MCP/REST coverage: all 604 operations across 57 categories indexed with params — covers per-service Docker network management, vault (external secrets) providers, DNS providers, forward-auth SSO domain protection, SCIM provisioning, build concurrency, and the auto-generated @dokploy/cli (604 commands incl. read-logs). Uses the official @dokploy/mcp server (secret fields are redacted by default since 0.30.0) plus debugging-focused slash commands including /compose-logs.

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
