# seo-dev (Codex plugin)

SEO development plugin for Agents Store. Technical SEO, structured data (JSON-LD), metadata API, Core Web Vitals, sitemaps, and content optimization patterns for Next.js App Router.

## Install

```bash
codex plugin marketplace add agents-store/seo-dev-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 11 skill(s) under `skills/` (includes 1 command(s) converted to skills — Codex has no custom slash-command system)
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- No MCP server
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/seo-dev
