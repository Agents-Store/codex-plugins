# codemap-dev (Codex plugin)

Code understanding plugin for developers. Helps onboard to unfamiliar projects through beginner-friendly code review, step-by-step explanations, visual diagrams (architecture, ERD, flows) via drawio-mcp, and frontend testing via Playwright MCP.

## Install

```bash
codex plugin marketplace add agents-store/codemap-dev-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 5 skill(s) under `skills/`
- 4 subagent(s) under `.codex/agents/`
- MCP server config in `.mcp.json`
- 7 workflow template(s) — see AGENTS.md

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/codemap-dev
