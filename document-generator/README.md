# document-generator (Codex plugin)

Professional document generator. Creates proposals, invoices, estimates/quotations, reports, presentations, contracts, NDAs, and certificates of completion in PDF, DOCX, and PPTX formats. Supports multi-language documents with embedded fonts (Cyrillic, Latin). First-use onboarding for style preferences, company profiles, and logo management. Converts between MD, DOCX, PDF, HTML, and PPTX.

## Install

```bash
codex plugin marketplace add agents-store/document-generator-codex
```

Or for local development:

```bash
codex plugin marketplace add .
```

## Components

- 16 skill(s) under `skills/` (includes 10 command(s) converted to skills — Codex has no custom slash-command system)
- 1 subagent definition(s) under `agents/` — **not installed automatically by Codex**. Copy manually:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- No MCP server
- No hooks

## Source

Canonical: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/document-generator
