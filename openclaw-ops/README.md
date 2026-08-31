# openclaw-ops (Codex plugin)

Operations plugin for a fleet of self-hosted OpenClaw gateway instances running as Docker Compose projects on one host. Discovers every instance from the live Docker state (never from hard-coded paths), classifies it ok/degraded/down/alien, and runs day-two maintenance: health and liveness reporting, provider-auth triage (expired, emptied and shadowed OAuth profiles, shared-credential token sink), config surgery with snapshot and executable rollback, memory/embedding repair and reindexing, shared skills and plugins consolidation, Infisical secret-delivery audit by key name only, security audit, version-drift and channel-aware upgrades, and reference-instance cloning. Mutations are dry-run by default behind an eight-block plan, need --yes, and need a typed confirmation when irreversible. Secrets are reported as fingerprints, presence and expiry — never as values. File-based knowledge: no MCP server, no required environment variables, no stored credentials; the single optional variable OPENCLAW_OPS_CONFIG is an escape hatch for the fleet-config path, and deployment specifics live in that operator-owned config outside the repository.

## Install

```bash
codex plugin marketplace add agents-store/openclaw-ops-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 23 skill(s) under `skills/` (includes 11 command(s) converted to skills — Codex has no custom slash-command system)
- 2 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- No MCP server
- Hooks in `hooks/hooks.json` — run `/hooks` after install to trust them

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/openclaw-ops
