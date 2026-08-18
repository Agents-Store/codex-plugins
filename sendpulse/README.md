# sendpulse (Codex plugin)

Sendpulse multi-channel marketing plugin. Manage chatbots (Telegram, WhatsApp, Instagram, Messenger, Viber), CRM (contacts, deals, pipelines, boards, tasks), email campaigns, templates, addressbooks, and SMTP transactional email via 133+ MCP tools.

## Install

```bash
codex plugin marketplace add agents-store/sendpulse-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 26 skill(s) under `skills/` (includes 15 command(s) converted to skills — Codex has no custom slash-command system)
- 2 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- MCP server config pointed to from the manifest (`.mcp.json`) — see AGENTS.md for the `~/.codex/config.toml` snippet
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/sendpulse
