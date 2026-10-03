# dataforseo-dev

> DataForSEO data for SEO work — keywords, SERP, backlinks, on-page, AI visibility — through the v3 MCP server. Not a general web-search tool.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/dataforseo-dev

## Skills

- **ai-optimization** — This skill should be used when the user asks about "AI optimization", "LLM mentions", "ChatGPT visibility", "AI search", "LLM ranking", "brand mentions in AI", "AI SEO", "GEO", "generative engine optimization", "AI Overview mentions", or needs to track and improve visibility in AI-powered search using DataForSEO.
- **api-reference** — This skill should be used when the user asks for "DataForSEO API endpoints", "DataForSEO REST API", "DataForSEO curl examples", "DataForSEO API documentation", "DataForSEO HTTP requests", or needs specific HTTP endpoint details for DataForSEO.
- **backlink-audit** — This skill should be used when the user asks about "backlink audit", "backlink analysis", "link profile", "toxic links", "referring domains", "link building", "backlink prospecting", "disavow list", "spam score", or needs to analyze backlink profiles using DataForSEO.
- **competitor-analysis** — This skill should be used when the user asks about "competitor analysis", "competitor research", "domain comparison", "who ranks for", "competitive landscape", "competitor keywords", "competitor traffic", or needs to analyze and compare domains using DataForSEO.
- **cost-awareness** — This skill should be used before any paid DataForSEO call, and when the user asks about "DataForSEO cost", "DataForSEO pricing", "how much will this cost", "DataForSEO budget", "credits", "billing", or wants to keep DataForSEO spending under control.
- **examples** — This skill should be used when the user asks for "DataForSEO examples", "DataForSEO workflows", "SEO analysis example", "show me how to use DataForSEO", or needs complete end-to-end scenario walkthroughs for SEO data analysis with DataForSEO.
- **keyword-research** — This skill should be used when the user asks about "keyword research", "find keywords", "keyword ideas", "search volume", "keyword difficulty", "long-tail keywords", "keyword gap analysis", "keyword strategy", or needs to discover and evaluate keywords using DataForSEO.
- **mcp-patterns** — This skill should be used when the user asks about "DataForSEO MCP tools", "api_request", "docs_search", "which DataForSEO endpoint", "how to call DataForSEO", "DataForSEO request body", "DataForSEO .ai mode", or needs to turn an SEO data task into the right DataForSEO API call through the MCP server.
- **setup** — This skill should be used when the user asks to "verify DataForSEO connection", "check DataForSEO MCP", "test DataForSEO setup", "is DataForSEO working", "set up DataForSEO credentials", or needs to confirm that the DataForSEO MCP integration is operational.
- **site-audit** — This skill should be used when the user asks about "site audit", "on-page audit", "lighthouse audit", "page speed", "technical SEO audit", "crawl site", "page analysis", "content analysis", "technology detection", or needs to analyze website pages and content using DataForSEO.
- **troubleshoot** — This skill should be used when the user encounters "DataForSEO errors", "DataForSEO not working", "DataForSEO connection issues", "debug DataForSEO", "DataForSEO MCP problems", "tool not found" for a DataForSEO tool, or needs to diagnose and fix common problems with the DataForSEO MCP server.
- **competitor-analysis** — Compare a domain against its competitors using DataForSEO
- **keyword-research** — Run keyword research for a topic or domain using DataForSEO
- **site-audit** — Run an on-page SEO audit for a URL using DataForSEO

## Subagents

Codex does not install plugin subagents automatically — copy them manually before use:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- **seo-data-analyst** — Use this agent when the user needs help with SEO data analysis using DataForSEO — keyword research, competitor analysis, backlink auditing, SERP monitoring, on-page audits, content analysis, or AI visibility tracking.

<example>
Context: User wants to find keyword opportunities for their product
user: "Research keywords for project management software. Find low-difficulty, high-volume opportunities."
assistant: "I'll use the seo-data-analyst agent to research keyword ideas, check difficulty and volume, and identify the best opportunities."
<commentary>
Keyword research workflow: Labs keyword_ideas, then keyword_overview, then bulk_keyword_difficulty and search_intent, filtered by difficulty and volume.
</commentary>
</example>

<example>
Context: User wants to understand their competitive landscape
user: "Compare my domain example.com against our top competitors for organic search"
assistant: "I'll use the seo-data-analyst agent to pull domain rank overviews, find competitor domains, and analyze keyword and backlink gaps."
<commentary>
Competitor analysis: Labs domain_rank_overview for each domain, competitors_domain, domain_intersection, then Backlinks bulk_ranks.
</commentary>
</example>

<example>
Context: User wants to audit their backlink profile
user: "Run a backlink audit on mysite.com and find any toxic or spammy links"
assistant: "I'll use the seo-data-analyst agent to analyze the full backlink profile, check spam scores, and identify problematic links."
<commentary>
Backlink audit: Backlinks summary, then backlinks, bulk_spam_score and anchors; flag a high spam score plus suspicious anchors.
</commentary>
</example>

<example>
Context: User wants to check brand visibility in AI responses
user: "How often does ChatGPT mention our brand when people ask about CRM software?"
assistant: "I'll use the seo-data-analyst agent to check LLM mention data for your brand across ChatGPT and Google AI Overviews and related queries."
<commentary>
AI visibility: LLM Mentions search_mentions and target_metrics for the brand, multi_target_metrics to compare with competitors, the ChatGPT LLM Scraper for a specific query.
</commentary>
</example>


## MCP servers

Add to `~/.codex/config.toml`:

```toml
[mcp_servers.dataforseo]
command = "npx"
args = ["-y","dataforseo-mcp-server@3"]

[mcp_servers.dataforseo.env]
"DATAFORSEO_USERNAME" = "${DATAFORSEO_USERNAME}"
"DATAFORSEO_PASSWORD" = "${DATAFORSEO_PASSWORD}"

```
