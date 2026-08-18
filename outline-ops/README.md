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
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- No MCP server
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/outline-ops
