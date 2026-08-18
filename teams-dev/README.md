# teams-dev (Codex plugin)

Microsoft Teams SDK dev plugin for Agents Store. TypeScript-first guidance for building Teams bots, message extensions, tabs, dialogs, and AI agents using @microsoft/teams.* packages — covers the App framework, activity routing, Adaptive Cards, AI/MCP/A2A, Microsoft Graph, SSO, sovereign clouds, the Teams CLI, devtools, sideloading, and end-to-end scenarios.

## Install

```bash
codex plugin marketplace add agents-store/teams-dev-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 20 skill(s) under `skills/` (includes 2 command(s) converted to skills — Codex has no custom slash-command system)
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- No MCP server
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/teams-dev
