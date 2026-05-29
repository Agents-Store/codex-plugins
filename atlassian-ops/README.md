# atlassian-ops (Codex plugin)

Atlassian Jira + Confluence Cloud ops plugin. Drive the full Jira Cloud REST API v3 and Confluence Cloud REST API v2 by curl — Jira: issues (create/edit/transition/assign, ADF bodies), JQL search, comments & worklogs, attachments & links, projects/versions/components, fields & screens, workflows/types/statuses, users & groups (accountId), permission/notification/security schemes, dashboards & filters, plans & teams; Confluence: pages & blog posts (versioned, storage/ADF bodies), spaces & permissions, comments & attachments, labels & content properties. Authenticates with Atlassian Cloud Basic auth (ATLASSIAN_EMAIL + ATLASSIAN_API_TOKEN) against ATLASSIAN_SITE_URL.

## Install

```bash
codex plugin marketplace add agents-store/atlassian-ops-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 6 skill(s) under `skills/`
- 1 subagent(s) under `.codex/agents/`
- No MCP server
- 0 workflow template(s) — see AGENTS.md

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/atlassian-ops
