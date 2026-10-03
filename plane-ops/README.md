# plane-ops (Codex plugin)

Plane Agile Ops knowledge plugin: sprint planning, task decomposition, estimation, backlog management, velocity tracking, retrospectives, standups, intake triage, modules, epics, initiatives, milestones, roadmaps, dependencies, burndown, pages (sprint reports, retros, release notes, ADRs, runbooks, specs, meeting notes), labels, workflow states, work item types, custom properties, comments, links, work logs, relations, history, bulk edits, search, members, and assignment. Written for Plane MCP 0.3.0 and later (one resource tool per entity with an `action` parameter, PQL filters, counts, releases); a bootstrap skill finds the tools under any server name and carries the fallback translation for older per-operation connectors. Ships no MCP server; the user connects Plane.

## Install

```bash
codex plugin marketplace add agents-store/plane-ops-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 61 skill(s) under `skills/` (includes 44 command(s) converted to skills — Codex has no custom slash-command system)
- 2 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- No MCP server
- Hooks in `hooks/hooks.json` — run `/hooks` after install to trust them

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/plane-ops
