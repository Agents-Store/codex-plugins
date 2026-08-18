# chatwoot-dev (Codex plugin)

Chatwoot dev plugin for Agents Store. Full REST API coverage (Application, Platform, and Public/Client APIs) with bundled OpenAPI specs, official chatwoot CLI recipes, webhook & agent-bot automation, and troubleshooting for developers building on Chatwoot. Authenticates with the api_access_token header via CHATWOOT_API_KEY against CHATWOOT_BASE_URL.

## Install

```bash
codex plugin marketplace add agents-store/chatwoot-dev-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 8 skill(s) under `skills/` (includes 2 command(s) converted to skills — Codex has no custom slash-command system)
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- No MCP server
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/chatwoot-dev
