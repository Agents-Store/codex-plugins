# dify-ops (Codex plugin)

Dify self-hosted update operations plugin. Pull upstream changes, merge into local dev branch, sync .env variables, detect Docker project names, and rebuild containers for Dify Docker deployments.

## Install

```bash
codex plugin marketplace add agents-store/dify-ops-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 4 skill(s) under `skills/`
- 1 subagent(s) under `.codex/agents/`
- No MCP server
- 2 workflow template(s) — see AGENTS.md

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/dify-ops
