# image-search-dev (Codex plugin)

Stock image and video search developer toolkit. MCP tool patterns for Pexels (9 tools) and Unsplash (4 tools) from mcpware-dev-tools. Photo search, video search, collections, curated content, and MinIO upload integration.

## Install

```bash
codex plugin marketplace add agents-store/image-search-dev-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 4 skill(s) under `skills/`
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- No MCP server
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/image-search-dev
