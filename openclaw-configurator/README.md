# openclaw-configurator (Codex plugin)

OpenClaw instance configurator and operations plugin. Scan, analyze, and optimize all workspace files (AGENTS.md, SOUL.md, USER.md, IDENTITY.md, TOOLS.md, HEARTBEAT.md, MEMORY.md, BOOT.md, BOOTSTRAP.md) plus openclaw.json. Update instances from official GitHub releases. Guided interviews, session log analysis, standing orders design, security audit, config validation, permission fix hooks, and 6 industry-specific workspace templates.

## Install

```bash
codex plugin marketplace add agents-store/openclaw-configurator-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 15 skill(s) under `skills/`
- 2 subagent(s) under `.codex/agents/`
- No MCP server
- 5 workflow template(s) — see AGENTS.md

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/openclaw-configurator
