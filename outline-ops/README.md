# outline-ops (Codex plugin)

Outline knowledge-base ops plugin. Drive the full Outline REST API by curl — documents (create, search, move, archive, trash, import/export, AI answers, memberships), collections (CRUD, user/group permissions, export), comments, stars, views, shares & access requests, users & groups, attachments & file operations, revisions, templates, events (audit log), OAuth clients, and data attributes. Authenticates with a Bearer OUTLINE_API_KEY against OUTLINE_API_URL.

## Install

```bash
codex plugin marketplace add agents-store/outline-ops-codex
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

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/outline-ops
