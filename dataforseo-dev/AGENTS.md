# dataforseo-dev

> DataForSEO data analysis plugin. Keyword research, competitor analysis, backlink auditing, SERP monitoring, on-page audits, content analysis, and AI optimization via 70+ MCP tools.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/dataforseo-dev

## MCP servers

Add to `~/.codex/config.toml`:

```toml
[mcp_servers.dataforseo]
command = "npx"
args = ["-y","dataforseo-mcp-server@latest"]

[mcp_servers.dataforseo.env]
"DATAFORSEO_USERNAME" = "${DATAFORSEO_USERNAME}"
"DATAFORSEO_PASSWORD" = "${DATAFORSEO_PASSWORD}"

```

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **ai-optimization** — This skill should be used when the user asks about "AI optimization", "LLM mentions", "ChatGPT visibility", "AI search", "LLM ranking", "brand mentions in AI", "AI SEO", "GEO", "generative engine optimization", or needs to track and improve visibility in AI-powered search using DataForSEO.
- **api-reference** — This skill should be used when the user asks for "DataForSEO API endpoints", "DataForSEO REST API", "DataForSEO curl examples", "DataForSEO API documentation", "DataForSEO HTTP requests", or needs specific HTTP endpoint details for DataForSEO.
- **backlink-audit** — This skill should be used when the user asks about "backlink audit", "backlink analysis", "link profile", "toxic links", "referring domains", "link building", "backlink prospecting", "disavow list", "spam score", or needs to analyze backlink profiles using DataForSEO.
- **competitor-analysis** — This skill should be used when the user asks about "competitor analysis", "competitor research", "domain comparison", "who ranks for", "competitive landscape", "competitor keywords", "competitor traffic", or needs to analyze and compare domains using DataForSEO.
- **examples** — This skill should be used when the user asks for "DataForSEO examples", "DataForSEO workflows", "SEO analysis example", "show me how to use DataForSEO", or needs complete end-to-end scenario walkthroughs for SEO data analysis with DataForSEO.
- **keyword-research** — This skill should be used when the user asks about "keyword research", "find keywords", "keyword ideas", "search volume", "keyword difficulty", "long-tail keywords", "keyword gap analysis", "keyword strategy", or needs to discover and evaluate keywords using DataForSEO.
- **mcp-patterns** — This skill should be used when the user asks about "DataForSEO MCP tools", "which DataForSEO tools are available", "how to use DataForSEO MCP", "DataForSEO tool parameters", "dataforseo tool list", or needs to know which MCP operations are available for DataForSEO and how to use them correctly.
- **setup** — This skill should be used when the user asks to "verify DataForSEO connection", "check DataForSEO MCP", "test DataForSEO setup", "is DataForSEO working", or needs to confirm that the DataForSEO MCP integration is operational.
- **site-audit** — This skill should be used when the user asks about "site audit", "on-page audit", "lighthouse audit", "page speed", "technical SEO audit", "crawl site", "page analysis", "content analysis", "technology detection", or needs to analyze website pages and content using DataForSEO.
- **troubleshoot** — This skill should be used when the user encounters "DataForSEO errors", "DataForSEO not working", "DataForSEO connection issues", "debug DataForSEO", "DataForSEO MCP problems", or needs to diagnose and fix common problems with DataForSEO MCP tools.

## Subagents

Defined under `.codex/agents/` as TOML files:

- **seo-data-analyst** — Use this agent when the user needs help with SEO data analysis using DataForSEO — keyword research, competitor analysis, backlink auditing, SERP monitoring, on-page audits, content analysis, or AI optimization tracking.

<example>
Context: User wants to find keyword opportunities for their product
user: "Research keywords for project management software. Find low-difficulty, high-volume opportunities."
assistant: "I'll use the seo-data-analyst agent to research keyword ideas, check difficulty and volume, and identify the best opportunities."
<commentary>
Keyword research workflow: keyword_ideas → keyword_overview → bulk_keyword_difficulty → search_intent, filtered by difficulty and volume.
</commentary>
</example>

<example>
Context: User wants to understand their competitive landscape
user: "Compare my domain example.com against our top competitors for organic search"
assistant: "I'll use the seo-data-analyst agent to pull domain rank overviews, find competitor domains, and analyze keyword and backlink gaps."
<commentary>
Competitor analysis: domain_rank_overview for each domain, competitors_domain, domain_intersection, backlinks_competitors.
</commentary>
</example>

<example>
Context: User wants to audit their backlink profile
user: "Run a backlink audit on mysite.com and find any toxic or spammy links"
assistant: "I'll use the seo-data-analyst agent to analyze the full backlink profile, check spam scores, and identify problematic links."
<commentary>
Backlink audit: backlinks_summary → backlinks_backlinks → bulk_spam_score → anchors, flag high spam score + suspicious anchors.
</commentary>
</example>

<example>
Context: User wants to check brand visibility in AI responses
user: "How often does ChatGPT mention our brand when people ask about CRM software?"
assistant: "I'll use the seo-data-analyst agent to check LLM mention data for your brand across AI models and related queries."
<commentary>
AI optimization: llm_ment_search for brand, llm_ment_agg_metrics, llm_ment_cross_agg_metrics to compare models.
</commentary>
</example>


## Workflows (canonical slash commands)

Codex CLI doesn't support custom slash commands — invoke these workflows via natural language. Each entry below is a prompt template you can adapt:

### `competitor-analysis`

Compare a domain against its competitors using DataForSEO

Arguments: `<domain>`

<details><summary>Prompt template</summary>

# Competitor Analysis

Run a comprehensive competitive analysis using DataForSEO MCP tools.

## Instructions

1. Load the competitor-analysis skill: invoke `/dataforseo-dev:competitor-analysis` mentally (use its workflow patterns)
2. Execute this workflow for the provided domain:
   - `mcp__dataforseo__dataforseo_labs_google_competitors_domain` with `exclude_top_domains: true` — discover competitors
   - `mcp__dataforseo__dataforseo_labs_google_domain_rank_overview` — get metrics for the target and top 3 competitors
   - `mcp__dataforseo__dataforseo_labs_google_domain_intersection` — find keyword gaps between target and top competitor
   - `mcp__dataforseo__backlinks_bulk_ranks` — compare backlink authority
3. Present results as a competitive landscape report:
   - **Domain Metrics Comparison**: Table comparing organic keywords, traffic, rank
   - **Top Competitors**: Ranked by relevance
   - **Keyword Gap Opportunities**: Keywords competitors rank for that the target doesn't
   - **Backlink Authority Gap**: Rank comparison
   - **Recommendations**: Quick win keywords to target, backlink strategies

## Default Parameters

- location_name: "United States" (ask user if different)
- language_code: "en" (ask user if different)
- exclude_top_domains: true
- limit: 20 for competitors, 50 for keyword gaps

</details>

### `keyword-research`

Run keyword research for a topic or domain using DataForSEO

Arguments: `<keyword or domain>`

<details><summary>Prompt template</summary>

# Keyword Research

Run a comprehensive keyword research workflow using DataForSEO MCP tools.

## Instructions

1. Load the keyword-research skill: invoke `/dataforseo-dev:keyword-research` mentally (use its workflow patterns)
2. Determine if the user provided a **keyword/topic** or a **domain**:
   - **Keyword/topic**: Use topic-based research workflow (keyword_ideas → keyword_overview → search_intent)
   - **Domain**: Use competitor-based research workflow (keywords_for_site → ranked_keywords → keyword_overview)
3. Execute the appropriate workflow from the keyword-research skill
4. Present results as a prioritized keyword table with columns: Keyword, Volume, Difficulty, CPC, Intent
5. Group results by search intent (informational, commercial, transactional, navigational)
6. Highlight quick wins (difficulty < 30, volume > 100)

## Default Parameters

- location_name: "United States" (ask user if different)
- language_code: "en" (ask user if different)
- limit: 50 for initial discovery, then filter

</details>

### `site-audit`

Run an on-page SEO audit for a URL using DataForSEO

Arguments: `<url>`

<details><summary>Prompt template</summary>

# Site Audit

Run a comprehensive on-page SEO audit using DataForSEO MCP tools.

## Instructions

1. Load the site-audit skill: invoke `/dataforseo-dev:site-audit` mentally (use its workflow patterns)
2. Run these tools in sequence on the provided URL:
   - `mcp__dataforseo__on_page_lighthouse` with `full_data: true` — get Lighthouse scores
   - `mcp__dataforseo__on_page_instant_pages` — get page-level SEO data
   - `mcp__dataforseo__on_page_content_parsing` — get structured content
3. If the user provided a domain (not a page URL), also run:
   - `mcp__dataforseo__domain_analytics_technologies_domain_technologies` — detect tech stack
4. Present results as a structured audit report:
   - **Performance Score**: Lighthouse performance (0-100)
   - **SEO Score**: Lighthouse SEO (0-100)
   - **Accessibility Score**: Lighthouse accessibility (0-100)
   - **Tech Stack**: Detected technologies
   - **Issues Found**: List of specific problems with severity
   - **Recommendations**: Prioritized fixes

## Default Parameters

- enable_javascript: true
- full_data: true (for Lighthouse)

</details>
