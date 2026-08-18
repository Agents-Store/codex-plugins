# firecrawl

> Firecrawl web scraping and search plugin. Scrape URLs, crawl sites, search the web, map site structures, extract structured data, batch scraping, autonomous research agents, and cloud browser sessions via MCP tools.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/firecrawl

## Skills

- **agent-research** — Autonomous research agent — start complex web research tasks, get structured results. This skill should be used when the user asks for multi-site exploration, open-ended web research, or deep-dive analysis across multiple sources.
- **batch-operations** — Batch scraping — scrape multiple URLs in a single request with rate limiting and progress tracking. This skill should be used when the user asks to scrape multiple pages at once, process a list of URLs, or bulk extract content.
- **browser-sessions** — Cloud browser sessions — create sessions, execute code, interact with dynamic pages. This skill should be used when the user needs to interact with SPAs, handle authentication flows, or execute browser automation on JavaScript-heavy sites.
- **crawling** — Multi-page crawling and site mapping. This skill should be used when the user asks to crawl an entire website, map all URLs on a site, discover site structure, or scrape multiple pages from a domain.
- **examples** — Tool call patterns, end-to-end workflow examples, and scenario references. This skill should be used when the user needs reference implementations, complete examples, or tool call patterns.
- **scraping** — Single URL scraping — formats, selectors, wait strategies, headers. This skill should be used when the user asks to scrape a web page, get content from a URL, convert a page to markdown, or extract text from a site.
- **search-extract** — Web search and structured data extraction. This skill should be used when the user asks to search the web, extract structured data from pages into JSON, research a topic using online sources, or perform competitive analysis.
- **agent-research** — Start an autonomous web research agent
- **batch-scrape** — Batch scrape multiple URLs at once
- **crawl-site** — Crawl a website
- **extract-data** — Extract structured data from URLs
- **map-site** — Map a site's URL structure
- **scrape-url** — Scrape a single URL
- **search** — Search the web

## Subagents

Codex does not install plugin subagents automatically — copy them manually before use:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- **firecrawl-assistant** — Interactive web scraping assistant. Scraping, crawling, batch operations, web search, data extraction, autonomous research, and cloud browser sessions.

<example>
user: "Scrape this webpage and extract the pricing data"
</example>
<example>
user: "Crawl the documentation site and summarize the content"
</example>
<example>
user: "Search the web for recent articles about AI regulations"
</example>


## MCP servers

Add to `~/.codex/config.toml`:

```toml
[mcp_servers.firecrawl]
url = "${FIRECRAWL_MCP_URL}"
type = "http"

```
