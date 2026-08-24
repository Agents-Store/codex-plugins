# macstack-dev (Codex plugin)

MACSTACK dev plugin for Agents Store. Creates and maintains the macstack/ folder of a Claude project: macstack.json — the standardized business + technical stack specification — plus the working documents around it (user cases per role, business logic in plain words, the decision log with cost-if-wrong, open questions split into what the client owes and what we deferred, and an immutable inbox for client material). Init in existing projects, generate from scratch (result-first), discover context plugins and prototypes, scaffold project files in the prototype → stack plugins → dev plugins order, merge incoming client edits through a gated delta/rulings loop, wire Infisical env, install best-practice rules and commands.

## Install

```bash
codex plugin marketplace add agents-store/macstack-dev-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 23 skill(s) under `skills/` (includes 9 command(s) converted to skills — Codex has no custom slash-command system)
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- No MCP server
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/macstack-dev
