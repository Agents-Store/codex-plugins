# taiga-ops (Codex plugin)

Taiga project-management ops plugin. Drive the full Taiga REST API by curl — projects, memberships, roles, milestones (sprints), epics, user stories, tasks, issues (with statuses, types, priorities, severities, points, custom attributes), wiki, history, attachments, comments, webhooks, notify policies, search, resolver, stats, and import/export. Authenticates with TAIGA_ADMIN_USERNAME + TAIGA_ADMIN_PASSWORD to obtain TAIGA_AUTH_TOKEN against TAIGA_API_URL.

## Install

```bash
codex plugin marketplace add agents-store/taiga-ops-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 5 skill(s) under `skills/`
- 1 subagent(s) under `.codex/agents/`
- No MCP server
- 0 workflow template(s) — see AGENTS.md

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/taiga-ops
