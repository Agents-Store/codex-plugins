# stack-ads

> Paid advertising stack for any project. Researches the product and its market, derives targeting, writes ad copy, generates images, video and voice-over, launches paused campaigns on Google Ads, Meta and TikTok from a reviewed dry-run plan with budget ceilings and a rollback ledger, and reports results. Walks the user through obtaining every key. Integrates Google Ads, Search Console, Meta, TikTok, Firecrawl, Apify, DataForSEO, OpenAI, Gemini and fal.ai via MCP and one dependency-free toolkit.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/stack-ads

## Skills

- **ad-copy** — This skill should be used when the user asks to "write ad copy", "write headlines and descriptions", "shorten these ad texts", "more ad text variants", or needs texts for Google, Meta or TikTok ads. Text only: for images and video use ad-creatives, for creating ads launch-campaign.
- **ad-creatives** — This skill should be used when the user asks to "generate ad creatives", "make ad images", "create a video for the ad", "add a voice-over", "resize for Stories", or needs media files for Google, Meta or TikTok ads. Not for ad text (ad-copy) or uploading (launch-campaign).
- **examples** — Complete walkthroughs of advertising a project with stack-ads, from getting the first key to a weekly review of running campaigns.
- **full-campaign** — This skill should be used when the user asks to "advertise this project", "run ads from scratch", "take this product from zero to running ads", "do the whole campaign end to end", or to continue an unfinished campaign checklist. For one step use that step's skill.
- **init-project** — This skill should be used when the user asks to "set up ads for this project", "connect Google Ads, Meta or TikTok", "where do I get the Meta, Google Ads or Firecrawl key", "set the ads budget ceiling", or when a key for advertising is missing. For the whole path use full-campaign.
- **launch-campaign** — This skill should be used when the user asks to "launch the campaign", "create the campaign in Google Ads, Meta or TikTok", "run the dry run", "activate, pause or roll back the campaign", or edit a live campaign. Not for budgets (media-plan), results (performance-report), full path (full-campaign).
- **market-research** — This skill should be used when the user asks to "research ad competitors", "find keywords for ads", "what are competitors advertising", "estimate CPC and search volume", "find interest ids for Meta or TikTok" before a media plan. Not for SEO audits or the product brief.
- **media-plan** — This skill should be used when the user asks to "build a media plan", "plan the ad campaign", "which platform should we advertise on", "how should we split the ad budget", "is this budget enough". Writes the plan only: for creating ads use launch-campaign, for results performance-report.
- **performance-report** — This skill should be used when the user asks "how are the ads performing", "make an ads report", "compare Google, Meta and TikTok results", "why is the cost per lead so high". Read-only: for API errors use troubleshoot, to change a campaign launch-campaign. Not for SEO reports.
- **product-brief** — This skill should be used when the user asks to "write the ad brief", "describe the product for advertising", "what do we sell and to whom", "extract the brand kit", or when ads/brief.md is missing. For competitors, keywords or platform audiences use market-research.
- **troubleshoot** — This skill should be used when the user reports "the ads command failed", "API error from Google Ads, Meta or TikTok", "my ad was rejected", "the key is rejected", "the MCP server is not connecting". Not for obtaining a key (init-project) or weak results (performance-report).

## Subagents

Codex does not install plugin subagents automatically — copy them manually before use:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- **stack-orchestrator** — Use this agent for advertising work that spans several steps or platforms and suits its own context: a research track, a creative batch, launch plans for several platforms, or a cross-platform report. It prepares and dry-runs; it never applies or activates a campaign.

<example>
Context: Research runs beside other work
user: "Research our competitors' ads and the keywords we should bid on, then give me the summary"
assistant: "I'll use the stack-orchestrator agent to run both research tracks and return the findings with their cost."
<commentary>Large API outputs stay out of the main conversation.</commentary>
</example>

<example>
Context: The media plan is approved
user: "Prepare the creatives and the launch plan files for Google, Meta and TikTok"
assistant: "I'll use the stack-orchestrator agent to build them and bring back the dry runs for your approval."
<commentary>It stops at the dry runs: applying a plan needs the user's approval.</commentary>
</example>


## MCP servers

Add to `~/.codex/config.toml`:

```toml
[mcp_servers.firecrawl]
url = "https://mcp.firecrawl.dev/v2/mcp"
type = "http"

[mcp_servers.firecrawl.headers]
"Authorization" = "Bearer ${FIRECRAWL_API_KEY}"

[mcp_servers.apify]
url = "https://mcp.apify.com"
type = "http"

[mcp_servers.apify.headers]
"Authorization" = "Bearer ${APIFY_TOKEN}"

[mcp_servers.dataforseo]
command = "npx"
args = ["-y","dataforseo-mcp-server@3"]

[mcp_servers.dataforseo.env]
"DATAFORSEO_USERNAME" = "${DATAFORSEO_USERNAME}"
"DATAFORSEO_PASSWORD" = "${DATAFORSEO_PASSWORD}"

[mcp_servers.google-ads]
command = "pipx"
args = ["run","--spec","google-ads-mcp==0.0.6","google-ads-mcp"]

[mcp_servers.google-ads.env]
"GOOGLE_CLOUD_PROJECT" = "${GOOGLE_CLOUD_PROJECT}"
"GOOGLE_APPLICATION_CREDENTIALS" = "${GOOGLE_APPLICATION_CREDENTIALS}"

[mcp_servers.meta-ads]
url = "https://mcp.facebook.com/ads"
type = "http"

[mcp_servers.meta-ads.headers]
"Authorization" = "Bearer ${META_ACCESS_TOKEN}"

[mcp_servers.tiktok-ads]
url = "https://business-api.tiktok.com/open_mcp/tt-ads-mcp-layer"
type = "http"

```

## Hooks

This plugin ships `hooks/hooks.json`. After installing, run `/hooks` to trust it before its hooks will run. Note: `PreToolUse.additionalContext` is currently ignored by Codex.
