# directus-dev (Codex plugin)

Directus development plugin. Knowledge base for working with Directus MCP tools (12 tools), REST API, and @directus/sdk. Covers collections, items, fields, relations, files, flows, operations, and schema design.

## Install

```bash
codex plugin marketplace add agents-store/directus-dev-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 20 skill(s) under `skills/` (includes 10 command(s) converted to skills — Codex has no custom slash-command system)
- 2 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- No MCP server
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/directus-dev
