# vaultwarden-dev (Codex plugin)

Vaultwarden dev plugin for Agents Store. Script and integrate a self-hosted Vaultwarden (Bitwarden-compatible) server: what the client API can and cannot do under end-to-end encryption, the identity token flows, organization member management, the /admin panel API, the Bitwarden CLI (bw, bw serve), python-vaultwarden and Terraform, client/server version compatibility, troubleshooting, and a guard hook that asks before a command prints decrypted secrets into the chat. File-based knowledge, no MCP, no stored credentials.

## Install

```bash
codex plugin marketplace add agents-store/vaultwarden-dev-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 10 skill(s) under `skills/` (includes 1 command(s) converted to skills — Codex has no custom slash-command system)
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- No MCP server
- Hooks in `hooks/hooks.json` — run `/hooks` after install to trust them

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/vaultwarden-dev
