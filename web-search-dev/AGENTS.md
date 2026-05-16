# web-search-dev

> Web search and scraping developer toolkit...

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/web-search-dev

## MCP servers

Add to `~/.codex/config.toml`:

```toml
[mcp_servers.firecrawl]
command = "npx"
args = ["-y","firecrawl-mcp"]

[mcp_servers.firecrawl.env]
"FIRECRAWL_API_KEY" = "${FIRECRAWL_API_TOKEN}"

[mcp_servers.exa]
command = "npx"
args = ["-y","exa-mcp-server"]

[mcp_servers.exa.env]
"EXA_API_KEY" = "${EXA_API_KEY}"

[mcp_servers.perplexity]
command = "npx"
args = ["-y","@perplexity-ai/mcp-server"]

[mcp_servers.perplexity.env]
"PERPLEXITY_API_KEY" = "${PERPLEXITY_API_KEY}"

[mcp_servers.jina]
url = "https://mcp.jina.ai/v1"
type = "http"

[mcp_servers.jina.headers]
"Authorization" = "Bearer ${JINA_API_KEY}"

[mcp_servers.context7]
command = "npx"
args = ["-y","@upstash/context7-mcp","--api-key","${CONTEXT7_API_KEY}"]

```

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **api-reference** — This skill should be used when the user asks for "Firecrawl API endpoints", "Exa REST API", "Perplexity API reference", "Jina API curl examples", "web search API documentation", or needs specific HTTP endpoint details for any of the web search and scraping services.
- **cli-recipes** — This skill should be used when the user asks about "firecrawl CLI", "jina CLI", "firecrawl command line", "jina command line", "scrape from terminal", "search from shell", or needs ready-to-use CLI commands for web scraping and search.
- **doc-search** — This skill should be used when the user asks to "find docs", "search documentation", "look up API reference", "find framework docs", "check Next.js docs", "search React docs", "find current docs for a library", or needs to find up-to-date documentation for a framework, library, or service while developing.
- **examples** — This skill should be used when the user asks for "web search examples", "scraping examples", "show me how to use search tools", "search workflow examples", or needs end-to-end scenario walkthroughs for web search and scraping development tasks.
- **mcp-patterns** — This skill should be used when the user asks about "web search MCP tools", "which search tools are available", "firecrawl tools", "exa tools", "jina tools", "perplexity tools", "how to use search MCP", "scraping MCP tools", "media search tools", or needs to know which MCP operations are available across web search and scraping services. Also triggers when doing any web research, URL fetching, or page content extraction — including during planning, exploration, or data source analysis.
- **media-search** — This skill should be used when the user asks to "find images", "search photos", "find stock photos", "search videos", "find media for app", "get stock images", "find pictures for website", or needs to find images, videos, or other media content for their application or website.
- **sdk-patterns** — This skill should be used when the user asks about "Firecrawl SDK", "Exa SDK", "exa-js", "exa-py", "Perplexity SDK", "Jina SDK", "web search client library", "search npm package", "search Python package", or needs code patterns for integrating web search services into a project.
- **setup** — This skill should be used when the user asks to "verify web search setup", "check search services", "test firecrawl connection", "test exa connection", "is jina working", "check perplexity MCP", or needs to confirm which web search and scraping services are operational.
- **troubleshoot** — This skill should be used when the user encounters "search not working", "scrape failing", "firecrawl error", "exa error", "jina error", "perplexity error", "rate limit", "401 unauthorized", "MCP connection failed", or needs to diagnose and fix problems with any web search or scraping service.
- **web-scraping** — This skill should be used when the user asks to "scrape a website", "extract content from URL", "crawl a site for data", "parse website content", "build a scraper", "extract product data", "scrape for my app", or needs to extract web content for their application or data pipeline. Also use when researching external data sources, analyzing websites for data availability, or fetching page content during planning — any task that involves reading web page content should use MCP scraping tools (firecrawl, jina, exa) instead of basic WebFetch.

## Subagents

Defined under `.codex/agents/` as TOML files:

- **web-search-developer** — Use this agent when the user needs help with web search and scraping tasks — extracting content from websites, finding documentation, searching for media, building data pipelines, or integrating search services (Firecrawl, Exa, Perplexity, Jina) into their project.

<example>
Context: User needs to scrape product data for their e-commerce app
user: "Help me scrape all product listings from this competitor's site and import them into my database"
assistant: "I'll use the web-search-developer agent to build the scraping pipeline."
<commentary>
Developer needs a multi-step scraping workflow: map site, extract structured data, transform, and import.
</commentary>
</example>

<example>
Context: User is debugging a framework issue and needs current docs
user: "I'm getting a hydration mismatch in Next.js 15 when formatting dates. Can you find the current docs on this?"
assistant: "I'll use the web-search-developer agent to search the latest Next.js documentation."
<commentary>
Developer needs up-to-date framework documentation to fix a version-specific bug.
</commentary>
</example>

<example>
Context: User building a blog needs stock images
user: "Find me landscape photos of modern office spaces for my blog's hero section"
assistant: "I'll use the web-search-developer agent to search stock photo services."
<commentary>
Developer needs media content for their application — search across Pexels, Unsplash, and web images.
</commentary>
</example>

