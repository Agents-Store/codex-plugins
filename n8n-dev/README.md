# n8n-dev (Codex plugin)

n8n workflow automation dev plugin for Agents Store. MCP tools guide (external + native), workflow patterns, expression syntax, validation, node configuration, Code node patterns, REST API reference, CLI recipes, and troubleshooting for developers building n8n workflows.

## Install

```bash
codex plugin marketplace add agents-store/n8n-dev-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 13 skill(s) under `skills/`
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- No MCP server
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/n8n-dev
