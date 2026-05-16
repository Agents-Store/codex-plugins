# codemap-dev (Codex plugin)

Code understanding plugin for developers. Helps onboard to unfamiliar projects through beginner-friendly code review, step-by-step explanations, and visual diagrams (architecture, ERD, flows) via drawio-mcp.

## Install

```bash
codex plugin marketplace add agents-store/codemap-dev-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 4 skill(s) under `skills/`
- 3 subagent(s) under `.codex/agents/`
- MCP server config in `.mcp.json`
- 6 workflow template(s) — see AGENTS.md

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/codemap-dev
