# mattermost-ops (Codex plugin)

Mattermost collaboration ops plugin. Drive the full Mattermost REST API v4 by curl — users, teams, channels (public/private/DM/group), posts & threads, reactions, files, custom emoji, webhooks, slash commands, bots, OAuth apps, plus system administration: config, RBAC roles & schemes, LDAP/SAML groups, compliance, data retention, plugins, jobs, and analytics. Authenticates with MATTERMOST_ADMIN_USERNAME + MATTERMOST_ADMIN_PASSWORD to obtain a session token against MATTERMOST_API_URL.

## Install

```bash
codex plugin marketplace add agents-store/mattermost-ops-codex
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

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/mattermost-ops
