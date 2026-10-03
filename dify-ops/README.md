# dify-ops (Codex plugin)

Dify self-hosted update operations plugin. Pre-flight the target release and the bundled Weaviate migration path, back up volumes with the stack down, merge a release tag into the local dev branch, sync .env variables, and pull and restart containers for Dify Docker deployments.

## Install

```bash
codex plugin marketplace add agents-store/dify-ops-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 6 skill(s) under `skills/` (includes 2 command(s) converted to skills — Codex has no custom slash-command system)
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- No MCP server
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/dify-ops
