# dataforseo-dev

> DataForSEO data for SEO work — keywords, SERP, backlinks, on-page, AI visibility — through the v3 MCP server. Not a general web-search tool.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/dataforseo-dev

## Skills

- **ai-optimization** — This skill should be used when the user asks about "AI optimization", "LLM mentions", "ChatGPT visibility", "AI search", "LLM ranking", "brand mentions in AI", "AI SEO", "GEO", "generative engine optimization", "AI Overview mentions", or needs to track and improve visibility in AI-powered search using DataForSEO.
- **api-reference** — This skill should be used when the user asks for "DataForSEO API endpoints", "DataForSEO REST API", "DataForSEO curl examples", "DataForSEO API documentation", "DataForSEO HTTP requests", or needs specific HTTP endpoint details for DataForSEO.
- **backlink-audit** — This skill should be used when the user asks about "backlink audit", "backlink analysis", "link profile", "toxic links", "referring domains", "link building", "backlink prospecting", "disavow list", "spam score", or needs to analyze backlink profiles using DataForSEO.
- **competitor-analysis** — Use for competitor SEO research with DataForSEO Labs data - who competes for the same organic keywords, who ranks in the top 10 for a query, how domains compare on organic traffic, keywords and trend, a competitor's top pages, and keyword or domain gaps. Not for keyword ideation (keyword-research), backlink audits (backlink-audit) or page audits (site-audit).
- **cost-awareness** — Use as the budget gate before the first paid DataForSEO api_request of a task, after the task skill has chosen its endpoints, and for any question about DataForSEO pricing - what a call costs, which calls are free, how to estimate or cap spend, how to check the account balance, and how to keep a bill small with bulk endpoints and explicit limit or depth.
- **examples** — This skill should be used when the user asks for "DataForSEO examples", "DataForSEO workflows", "SEO analysis example", "show me how to use DataForSEO", or needs complete end-to-end scenario walkthroughs for SEO data analysis with DataForSEO.
- **keyword-research** — This skill should be used when the user asks about "keyword research", "find keywords", "keyword ideas", "search volume", "keyword difficulty", "long-tail keywords", "keyword gap analysis", "keyword strategy", or needs to discover and evaluate keywords using DataForSEO.
- **mcp-patterns** — Use for how to call the DataForSEO MCP server - its tools (api_request, docs_search, docs_index, docs_list_sections), the docs-first cycle, request bodies and the .ai response mode - and to build the right call for an endpoint family without a task skill, such as SERP, Merchant, App Data or Business Data. Not for site audits, content analysis, page parsing, technologies or WHOIS (site-audit).
- **setup** — This skill should be used when the user asks to "verify DataForSEO connection", "check DataForSEO MCP", "test DataForSEO setup", "is DataForSEO working", "set up DataForSEO credentials", or needs to confirm that the DataForSEO MCP integration is operational.
- **site-audit** — Use for page and site inspection with DataForSEO OnPage, Content Analysis and Domain Analytics - Lighthouse and page speed, on-page SEO checks (title, meta, headings, alt text), site crawls, parsing a page's content, content and sentiment analysis for a keyword, the technologies a domain runs, and WHOIS data. Not for backlinks (backlink-audit), competitor comparison (competitor-analysis) or keyword research.
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
