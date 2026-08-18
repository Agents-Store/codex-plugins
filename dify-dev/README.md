# dify-dev (Codex plugin)

Dify API dev plugin for Agents Store. Complete coverage of the Dify App Service API (chat, completion, workflows, conversations, files, audio, annotations) and the Knowledge Base / Datasets API — auth, endpoints, streaming events, curl examples, and troubleshooting.

## Install

```bash
codex plugin marketplace add agents-store/dify-dev-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 12 skill(s) under `skills/` (includes 3 command(s) converted to skills — Codex has no custom slash-command system)
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- No MCP server
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/dify-dev
