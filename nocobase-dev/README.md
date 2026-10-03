# nocobase-dev (Codex plugin)

NocoBase v2 development plugin. Build, manage, and operate NocoBase through the `nb` CLI 2.2 (primary) or REST API (fallback). Bundles 20 official upstream skills from nocobase/skills (auto-synced weekly via GitHub Action; UI authoring enters through nocobase-portal-manage), 5 custom skills (overview, auth, cli-recipes, api-reference, examples), and an OpenAPI 3.0.3 snapshot of NocoBase v2.1.0-beta.29 (272 endpoints; refresh from a 2.2.x stand pending). Targets the stable @nocobase/cli channel (Node.js 22+); NocoBase 2.1.0+ for agent connection. No MCP server shipped; NocoBase has its own at /api/mcp. Env vars match upstream naming: NB_URL + NB_USER + NB_PASSWORD for sign-in flow, or NB_URL + NB_TOKEN for the long-lived API Key path.

## Install

```bash
codex plugin marketplace add agents-store/nocobase-dev-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 25 skill(s) under `skills/`
- No MCP server
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/nocobase-dev
