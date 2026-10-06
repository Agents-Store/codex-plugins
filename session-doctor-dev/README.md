# session-doctor-dev (Codex plugin)

Read-only audit of a Claude Code session: where it runs, what context it loaded, which model and effort it used, the skills it invoked, every HTTP request with its status code, and the status of each API token seen in the session — with concrete fixes.

## Install

```bash
codex plugin marketplace add agents-store/session-doctor-dev-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 2 skill(s) under `skills/`
- No MCP server
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/session-doctor-dev
