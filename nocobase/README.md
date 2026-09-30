# nocobase (Codex plugin)

DEPRECATED — superseded by nocobase-dev, which bundles the official nocobase/skills library and works through the nb CLI and REST API. Its MCP server package (@nocobase/mcp-server) was withdrawn from npm and is no longer wired; the MCP commands and agents work only if you register your own server named `nocobase`. NocoBase platform development plugin. Expert guidance on collections, fields, relations, workflows, UI blocks, plugin development, MCP-powered page management, data operations, and collection inspection for NocoBase applications.

## Install

```bash
codex plugin marketplace add agents-store/nocobase-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 15 skill(s) under `skills/` (includes 8 command(s) converted to skills — Codex has no custom slash-command system)
- 2 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- No MCP server
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/nocobase
