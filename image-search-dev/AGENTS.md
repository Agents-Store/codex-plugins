# image-search-dev

> Stock image and video search developer toolkit. MCP tool patterns for Pexels (9 tools) and Unsplash (4 tools) from mcpware-dev-tools. Photo search, video search, collections, curated content, and MinIO upload integration.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/image-search-dev

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **examples** — This skill should be used when the user asks for "image search examples", "stock photo workflow", "show me how to find images", "media search walkthrough", "pexels usage example", "unsplash usage example", or wants to see end-to-end scenarios for finding and using stock images and videos.
- **mcp-patterns** — This skill should be used when the user asks about "image search MCP tools", "pexels tools", "unsplash tools", "which image search tools are available", "how to find stock photos", "search photos MCP", "stock image tools", "pexels parameters", "unsplash parameters", or needs to know which MCP operations are available for searching stock images and videos.
- **setup** — This skill should be used when the user asks to "verify image search setup", "check pexels connection", "check unsplash MCP", "test image search tools", "is pexels working", "is unsplash working", or needs to confirm that the Pexels and Unsplash MCP tools are operational.
- **troubleshoot** — This skill should be used when the user encounters "pexels error", "unsplash error", "image search not working", "rate limit on stock photos", "stock photo tool not found", "image search rate limited", or needs to diagnose and fix problems with Pexels, Unsplash, or MinIO upload tools.

## Subagents

Defined under `.codex/agents/` as TOML files:

- **image-search-developer** — Use this agent when the user needs help finding stock images or videos — searching Pexels and Unsplash, browsing collections, selecting the right image sizes, or integrating stock media into their application.

<example>
Context: User needs hero images for a website
user: "Find me some high-quality landscape photos for a tech startup landing page"
assistant: "I'll use the image-search-developer agent to search for suitable hero images."
<commentary>
User needs to find stock photos for a specific use case — agent can search both Pexels and Unsplash.
</commentary>
</example>

<example>
Context: User needs video content
user: "I need a short background video for my landing page hero section"
assistant: "I'll use the image-search-developer agent to find background videos on Pexels."
<commentary>
User needs stock video content — only Pexels supports video search.
</commentary>
</example>

<example>
Context: User wants to build a media gallery
user: "Help me find and organize photos for a travel blog gallery"
assistant: "I'll use the image-search-developer agent to search and curate the gallery content."
<commentary>
User needs multiple images organized by theme — agent can browse collections and search by topic.
</commentary>
</example>

