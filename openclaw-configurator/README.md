# openclaw-configurator (Codex plugin)

OpenClaw instance configurator and operations plugin. Scan, analyze, and optimize all workspace files (AGENTS.md, SOUL.md, USER.md, IDENTITY.md, TOOLS.md, HEARTBEAT.md, MEMORY.md, BOOT.md, BOOTSTRAP.md) plus openclaw.json. Update instances from official GitHub releases and reconcile config against new releases (recommend new features, migrate legacy settings, run openclaw doctor). Set up model-provider authentication with an OAuth/CLI-backend-first, cost-saving bias (Claude CLI, Codex OAuth). Migrate .env secrets into self-hosted Infisical. Guided interviews, session log analysis, standing orders design, security audit, config validation, permission fix hooks, centralized doc research, and 6 industry-specific workspace templates.

## Install

```bash
codex plugin marketplace add agents-store/openclaw-configurator-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 26 skill(s) under `skills/` (includes 7 command(s) converted to skills — Codex has no custom slash-command system)
- 2 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- No MCP server
- Hooks in `hooks/hooks.json` — run `/hooks` after install to trust them

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/openclaw-configurator
