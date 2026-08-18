# macstack-dev (Codex plugin)

MACSTACK dev plugin for Agents Store. Creates and maintains macstack.json — the standardized business + technical stack specification for Claude projects: init in existing projects, generate from scratch (result-first), discover context plugins and prototypes, scaffold project files in the prototype → stack plugins → dev plugins order, wire Infisical env, install best-practice rules and commands.

## Install

```bash
codex plugin marketplace add agents-store/macstack-dev-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 17 skill(s) under `skills/` (includes 6 command(s) converted to skills — Codex has no custom slash-command system)
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- No MCP server
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/macstack-dev
