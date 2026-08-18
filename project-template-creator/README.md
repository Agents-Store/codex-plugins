# project-template-creator (Codex plugin)

Manage project template hierarchy with unified improvement workflow. Route fixes to plugins or parent templates automatically, quick-capture ideas for later, and run unified end-of-session reviews covering both plugins and templates.

## Install

```bash
codex plugin marketplace add agents-store/project-template-creator-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 18 skill(s) under `skills/` (includes 8 command(s) converted to skills — Codex has no custom slash-command system)
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- No MCP server
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/project-template-creator
