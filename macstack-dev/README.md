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

- 11 skill(s) under `skills/`
- 1 subagent(s) under `.codex/agents/`
- No MCP server
- 6 workflow template(s) — see AGENTS.md

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/macstack-dev
