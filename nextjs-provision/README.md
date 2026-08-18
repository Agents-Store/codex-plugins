# nextjs-provision (Codex plugin)

Next.js provisioning plugin. Set up shadcn/ui and shadcn studio — component installation, theme configuration, MCP server setup, project scaffolding, and multi-registry component search across 260+ registries from the official directory.

## Install

```bash
codex plugin marketplace add agents-store/nextjs-provision-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 11 skill(s) under `skills/` (includes 3 command(s) converted to skills — Codex has no custom slash-command system)
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- MCP server config pointed to from the manifest (`.mcp.json`) — see AGENTS.md for the `~/.codex/config.toml` snippet
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/nextjs-provision
