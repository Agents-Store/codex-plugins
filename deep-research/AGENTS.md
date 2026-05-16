# deep-research

> Deep Research plugin. Comprehensive web research using 4 providers (Exa, Firecrawl, Jina, Perplexity) with capability-based CONNECTORS pattern and automatic FALLBACK chains. Search, scrape, crawl, extract — each action tries multiple providers until one succeeds.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/deep-research

## MCP servers

Add to `~/.codex/config.toml`:

```toml
[mcp_servers.deep-research]
url = "https://mcp.mcpware.net/mcp/d1ab2e49-a032-4788-9ba5-3125624f2769"
type = "http"

```

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **content-extraction** — Content reading and extraction guidelines — reading URLs, scraping pages, crawling sites, extracting PDFs, and taking screenshots. Use when reading web content, extracting structured data from pages, or processing documents.
- **deep-research** — Main research automation skill. 7-step algorithm for comprehensive research with 6 research types, query planning, parallel search, extraction, synthesis, and structured reporting. Use when conducting any multi-step research task.
- **examples** — Tool call patterns, end-to-end research workflow examples, and scenario references for all 6 research types. Use when you need reference implementations or complete research examples.
- **report-generation** — Report templates and generation guidelines — Executive Summary, Deep Research Report, and Comparison Table formats with methodology and citation rules. Use when formatting research results into structured reports.
- **search-strategies** — Search strategy guidelines — tool selection, fallback chains, query optimization, and parallel search orchestration. Use when choosing which search tools to use, handling tool failures, or optimizing search queries.

## Workflows (canonical slash commands)

Codex CLI doesn't support custom slash commands — invoke these workflows via natural language. Each entry below is a prompt template you can adapt:

### `compare`

Compare multiple products, companies, or technologies

Arguments: `<item1> vs <item2> [vs <item3>...] [--criteria <criteria>]`

<details><summary>Prompt template</summary>

# Compare

Comparative analysis of multiple items using the Comparison Table template. See CONNECTORS.md for provider mapping.

## Process

1. **Parse items** from arguments (split by "vs"):
   ```
   Items: [item1, item2, item3...]
   ```

2. **Search for each item** + comparison articles:
   ```
   ~~batch_search([
     "{item1} features pricing review",
     "{item2} features pricing review",
     "{item1} vs {item2} comparison"
   ])

   ~~search("{item1} vs {item2} vs {item3} comparison")
   ```

3. **Read and extract** from pricing/feature pages:
   ```
   ~~extract(
     urls: [item_pricing_urls],
     prompt: "Extract pricing plans and key features",
     schema: { plans with name, price, features }
   )

   ~~batch_scrape(comparison_article_urls)
   ```

4. **Synthesize** into Comparison Table:
   ```
   Deduplicate facts
   Build feature comparison matrix
   Analyze strengths/weaknesses per item
   ```

5. **Output** Comparison Table report with:
   - Feature comparison table
   - Detailed analysis per item
   - Verdict by use case
   - Sources + Methodology

## Example Usage
```
/compare Notion vs Linear vs Asana
/compare PostgreSQL vs MySQL vs MongoDB --criteria performance, scalability, ease of use
/compare React vs Vue vs Svelte
```

</details>

### `crawl-site`

Crawl an entire website or map its structure

Arguments: `<url> [--depth <number>] [--limit <max-pages>] [--map-only]`

<details><summary>Prompt template</summary>

# Crawl Site

Crawl a website to extract all pages or map its structure. See CONNECTORS.md for provider mapping.

## Process

1. **If --map-only:** get site structure only:
   ```
   ~~crawl(url, map_only: true)
   → Returns list of all URLs
   ```

2. **Otherwise:** full crawl with monitoring (~~crawl):
   ```
   ~~crawl(url, limit: <--limit or 20>, depth: <--depth or 3>)
   → Poll status until completed
   ```

3. **On timeout/error:** fallback to map + selective read:
   ```
   ~~crawl(url, map_only: true) → get URLs
   ~~batch_scrape(key_page_urls) → read selected pages
   ```

4. **Display** crawl results summary with page count and key content.

## Example Usage
```
/crawl-site https://docs.example.com
/crawl-site https://docs.example.com --limit 50 --depth 4
/crawl-site https://example.com --map-only
```

</details>

### `read-url`

Read and extract content from a URL with fallback

Arguments: `<url> [--format <markdown|json|screenshot>]`

<details><summary>Prompt template</summary>

# Read URL

Read and extract content from a web page or PDF. See CONNECTORS.md for provider mapping.

## Process

1. **Detect content type** from URL:
   - `.pdf` → use PDF extraction
   - Regular URL → use ~~scrape with fallback

2. **Read content** with fallback (~~scrape):
   ```
   Try each provider: Jina → Firecrawl
   On error → next provider automatically
   ```

3. **Optional extras:**
   - `--format screenshot` → capture page screenshot
   - `--format json` → structured JSON extraction
   - Check publish date → detect page date

4. **Display** extracted content with source URL and date.

## Example Usage
```
/read-url https://example.com/article
/read-url https://arxiv.org/pdf/2301.00001
/read-url https://spa-app.com/page --format screenshot
```

</details>

### `research`

Conduct comprehensive deep research on a topic using 7-step algorithm

Arguments: `<topic> [--type <competitive|market|technical|person|topic|news>] [--depth <quick|standard|deep>]`

<details><summary>Prompt template</summary>

# Deep Research

Execute the full 7-step research algorithm from the `deep-research` skill. See CONNECTORS.md for provider mapping and fallback chains.

## Steps

1. **CLASSIFY** — determine research type from topic (or use --type flag)
2. **PLAN** — expand query, generate 3-7 search queries from different angles
3. **SEARCH** — ~~batch_search / ~~search with fallback
4. **READ** — ~~batch_scrape / ~~scrape top-5 pages with fallback
5. **EXTRACT** — key facts, data, quotes with source URLs
6. **SYNTHESIZE** — deduplicate, cross-check, assess confidence
7. **REPORT** — structured report using appropriate template + Methodology

For full algorithm details, query patterns per type, and quality checklist — see the `deep-research` skill.

## Example Usage
```
/research AI code assistants market 2026
/research Notion vs Linear vs Asana --type competitive
/research RAG pipeline architecture --type technical --depth deep
/research latest AI regulation news --type news
```

</details>

### `search`

Search the web using optimal provider with automatic fallback

Arguments: `<query> [--type <web|code|academic>]`

<details><summary>Prompt template</summary>

# Web Search

Search the web with automatic fallback between providers. See CONNECTORS.md for provider mapping.

## Process

1. **Determine search type** from query or --type flag:
   - `web` (default): general web search
   - `code`: code and technical search
   - `academic`: arxiv and papers

2. **Execute search** with fallback chain:

   **Web search (~~search):**
   ```
   Try each provider: Exa → Perplexity → Jina → Firecrawl
   On error → next provider automatically
   ```

   **Code search (~~code_search):**
   ```
   Try: Exa code → ~~search + "github code"
   ```

   **Academic search (~~academic_search):**
   ```
   Try: Jina arXiv → Jina SSRN → Perplexity + "paper"
   ```

3. **Display results** with titles, URLs, and snippets.

## Example Usage
```
/search best RAG frameworks 2026
/search React server components --type code
/search transformer attention mechanism --type academic
```

</details>

### `summarize`

Summarize a topic or URL content

Arguments: `<topic-or-url>`

<details><summary>Prompt template</summary>

# Summarize

Quick summary of a topic or URL content. See CONNECTORS.md for provider mapping.

## Process

1. **Detect input type:**
   - Starts with `http` → URL to summarize
   - Otherwise → topic to research and summarize

2. **For URL input:**
   ```
   ~~scrape(url) → get content
   Classify content type (news, tutorial, research, etc.)
   Summarize: key points, main argument, data highlights
   ```

3. **For topic input:**
   ```
   ~~search(topic) → AI-summarized answer + top results
   ~~scrape(top_result_url) → fuller context if needed
   ```

4. **Output** concise summary with:
   - Key points (3-5 bullets)
   - Main insights
   - Source URLs

## Example Usage
```
/summarize https://example.com/long-article
/summarize transformer architecture
/summarize latest developments in quantum computing
```

</details>
