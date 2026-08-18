# media-hosting-ops (Codex plugin)

Media hosting operations plugin. Upload images by public URL to MinIO-based media hosting via the uploadImageToMinio MCP tool.

## Install

```bash
codex plugin marketplace add agents-store/media-hosting-ops-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 2 skill(s) under `skills/`
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- No MCP server
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/media-hosting-ops
