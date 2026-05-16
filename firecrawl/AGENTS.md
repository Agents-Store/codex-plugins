# firecrawl

> Firecrawl web scraping and search plugin. Scrape URLs, crawl sites, search the web, map site structures, extract structured data, batch scraping, autonomous research agents, and cloud browser sessions via MCP tools.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/firecrawl

## MCP servers

Add to `~/.codex/config.toml`:

```toml
[mcp_servers.firecrawl]
url = "${FIRECRAWL_MCP_URL}"
type = "http"

```

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **agent-research** — Autonomous research agent — start complex web research tasks, get structured results. This skill should be used when the user asks for multi-site exploration, open-ended web research, or deep-dive analysis across multiple sources.
- **batch-operations** — Batch scraping — scrape multiple URLs in a single request with rate limiting and progress tracking. This skill should be used when the user asks to scrape multiple pages at once, process a list of URLs, or bulk extract content.
- **browser-sessions** — Cloud browser sessions — create sessions, execute code, interact with dynamic pages. This skill should be used when the user needs to interact with SPAs, handle authentication flows, or execute browser automation on JavaScript-heavy sites.
- **crawling** — Multi-page crawling and site mapping. This skill should be used when the user asks to crawl an entire website, map all URLs on a site, discover site structure, or scrape multiple pages from a domain.
- **examples** — Tool call patterns, end-to-end workflow examples, and scenario references. This skill should be used when the user needs reference implementations, complete examples, or tool call patterns.
- **scraping** — Single URL scraping — formats, selectors, wait strategies, headers. This skill should be used when the user asks to scrape a web page, get content from a URL, convert a page to markdown, or extract text from a site.
- **search-extract** — Web search and structured data extraction. This skill should be used when the user asks to search the web, extract structured data from pages into JSON, research a topic using online sources, or perform competitive analysis.

## Subagents

Defined under `.codex/agents/` as TOML files:

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


## Workflows (canonical slash commands)

Codex CLI doesn't support custom slash commands — invoke these workflows via natural language. Each entry below is a prompt template you can adapt:

### `agent-research`

Start an autonomous web research agent

Arguments: `<research-prompt> [--urls <url1,url2>]`

<details><summary>Prompt template</summary>

# Agent Research

Start an autonomous research agent that browses, searches, and extracts information based on a natural language task description.

## Arguments
Format: `<research-prompt> [--urls <url1,url2>]`
- research-prompt: Description of the research task (required, max 10,000 chars)
- --urls: Starting URLs to guide the agent, comma-separated (optional)

Parse from "$ARGUMENTS".

## Process

1. **Start research agent:**
   Use `agent` with the research prompt and optional starting URLs.

2. **Monitor progress:**
   Use `agent_status` to poll until the agent completes.

3. **Present results:**
   Show the agent's findings in a structured format.

## Example Usage
```
/agent-research "Compare pricing plans for the top 5 CRM tools"
/agent-research "Analyze the API documentation of Stripe vs PayPal" --urls https://docs.stripe.com,https://developer.paypal.com
/agent-research "Find the latest AI automation tools launched in the last month"
```

</details>

### `batch-scrape`

Batch scrape multiple URLs at once

Arguments: `<url1> <url2> [url3] ... [--format <markdown|html>]`

<details><summary>Prompt template</summary>

# Batch Scrape

Scrape multiple URLs in a single request with rate limiting and progress tracking.

## Arguments
Format: `<url1> <url2> [url3] ... [--format <markdown|html>]`
- urls: One or more URLs to scrape (required, space-separated)
- --format: Output format — markdown (default) or html (optional)

Parse from "$ARGUMENTS".

## Process

1. **Start batch scrape:**
   Use `batch_scrape` with the list of URLs and format options.

2. **Monitor progress:**
   Use `check_batch_status` to poll until complete.

3. **Present results:**
   Show summary of scraped content for each URL.

## Example Usage
```
/batch-scrape https://example.com/page1 https://example.com/page2 https://example.com/page3
/batch-scrape https://docs.example.com/intro https://docs.example.com/guide --format markdown
```

</details>

### `crawl-site`

Crawl a website

Arguments: `<url> [--max-pages <n>] [--include <pattern>]`

<details><summary>Prompt template</summary>

# Crawl Site

Start crawling a website and monitor progress.

## Arguments
Format: `<url> [--max-pages <n>] [--include <pattern>]`
- url: Starting URL to crawl (required)
- --max-pages: Maximum pages to crawl, default 50 (optional)
- --include: URL pattern to include, e.g. "/blog/*" (optional)

Parse from "$ARGUMENTS".

## Process

1. **Start crawl:**
   ```
   crawl_site({
     url: <url>,
     max_pages: <max or 50>,
     include_patterns: [<pattern>]
   })
   ```

2. **Monitor progress:**
   ```
   get_crawl_status(crawl_id)
   ```
   Report pages crawled, status, and when complete.

3. **Display results summary:**
   Show total pages crawled, URLs found, and content summary.

## Example Usage
```
/crawl-site "https://example.com"
/crawl-site "https://example.com" --max-pages 100 --include "/docs/*"
```

</details>

### `extract-data`

Extract structured data from URLs

Arguments: `<url> [url2...] --schema <json-schema>`

<details><summary>Prompt template</summary>

# Extract Data

Extract structured data from a URL using a JSON schema definition.

## Arguments
Format: `<url> [url2...] --schema <json-schema>`
- urls: One or more URLs to extract data from (required)
- --schema: JSON schema describing the data to extract (required)

Parse from "$ARGUMENTS".

## Process

1. **Extract data:**
   ```
   extract_data({
     urls: [<url>, ...],
     schema: <parsed JSON schema>
   })
   ```

2. **Display results:**
   Show extracted data in a formatted table.

## Example Usage
```
/extract-data "https://example.com/product/123" --schema '{"type":"object","properties":{"name":{"type":"string"},"price":{"type":"number"},"description":{"type":"string"}}}'
```

</details>

### `map-site`

Map a site's URL structure

Arguments: `<url>`

<details><summary>Prompt template</summary>

# Map Site

Discover all URLs on a website.

## Arguments
Format: `<url>`
- url: Site URL to map (required)

Parse from "$ARGUMENTS".

## Process

1. **Map the site:**
   ```
   map_site({ url: <url> })
   ```

2. **Present results:**
   - Total URL count
   - URL tree organized by path segments
   - Key sections identified (blog, docs, pricing, etc.)
   - Suggest include/exclude patterns for focused crawling

3. **Suggest next steps:**
   - Use `/crawl-site` with include patterns for specific sections
   - Use `/batch-scrape` for a curated list of important URLs

## Example Usage
```
/map-site "https://example.com"
/map-site "https://docs.stripe.com"
```

</details>

### `scrape-url`

Scrape a single URL

Arguments: `<url> [--format <markdown|html>]`

<details><summary>Prompt template</summary>

# Scrape URL

Scrape a single URL and return its content.

## Arguments
Format: `<url> [--format <markdown|html>]`
- url: The URL to scrape (required)
- --format: Output format — markdown (default) or html (optional)

Parse from "$ARGUMENTS".

## Process

1. **Scrape the URL:**
   ```
   scrape_url({
     url: <url>,
     formats: [<format or "markdown">]
   })
   ```

2. **Display results:**
   Show page title, content summary, and metadata.

## Example Usage
```
/scrape-url "https://example.com/blog/article"
/scrape-url "https://example.com/products/123" --format html
```

</details>

### `search`

Search the web

Arguments: `<query> [--limit <n>]`

<details><summary>Prompt template</summary>

# Search

Search the web and get results with page content.

## Arguments
Format: `<query> [--limit <n>]`
- query: Search query (required)
- --limit: Max results, default 5 (optional)

Parse from "$ARGUMENTS".

## Process

1. **Search:**
   ```
   search({
     query: <query>,
     limit: <limit or 5>,
     lang: <language if specified>,
     country: <country if specified>
   })
   ```

2. **Display results:**
   Show title, URL, and content snippet for each result in a formatted table.

3. **Suggest follow-ups:**
   - Scrape specific results for full content
   - Extract structured data from results
   - Narrow search with filters

## Advanced Options
- `--lang <code>` — filter by language (e.g., "en", "de", "ru")
- `--country <code>` — filter by country (e.g., "us", "uk", "de")

## Example Usage
```
/search "best practices for API design"
/search "n8n workflow examples" --limit 10
/search "machine learning frameworks 2024" --lang en --country us
```

</details>
