# n8n-provision (Codex plugin)

n8n instance provisioning plugin. Discover workflows from the official template library (9,166+ templates), GitHub repos, and community platforms, then analyze, import, and batch-deploy them to provision an n8n instance.

## Install

```bash
codex plugin marketplace add agents-store/n8n-provision-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 14 skill(s) under `skills/` (includes 5 command(s) converted to skills — Codex has no custom slash-command system)
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- No MCP server
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/n8n-provision
