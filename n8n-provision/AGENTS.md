# n8n-provision

> n8n instance provisioning plugin. Discover workflows from the official template library (9,166+ templates), GitHub repos, and community platforms, then analyze, import, and batch-deploy them to provision an n8n instance.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/n8n-provision

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **batch-provisioning** — Provision an n8n instance with multiple workflows in a single batch operation. Handles dependency ordering, credential grouping, progress tracking, rollback strategy, and post-flight verification.
Use when: "provision n8n instance", "batch import workflows", "set up n8n with workflows", "deploy multiple templates", "bootstrap n8n", "install workflow suite", "bulk deploy workflows"

- **community-source-discovery** — Find n8n workflows from GitHub repositories and third-party platforms when the official template library is insufficient. Use when asked to "find n8n workflow on github", "community n8n workflows", "search github for n8n", "n8n workflow sources", "n8n automation repository", or when official template search returns no good matches.
- **credential-planning** — Plan and prepare credentials before importing workflows to an n8n instance. Extracts credential requirements from workflow JSON, checks existing credentials, groups by service, and produces a setup checklist.
Use when: "plan credentials for import", "what credentials needed", "n8n credential setup", "prepare credentials before import", "credential requirements", "which credentials do I need", "credential checklist"

- **examples** — End-to-end scenario walkthroughs for n8n workflow provisioning. This skill should be used when the user asks for "n8n provisioning example", "how to import workflows", "n8n provision walkthrough", "batch import tutorial", "template deploy guide", "n8n setup example", "find and import n8n workflow", "deploy automation suite", or needs a complete example of discovering, analyzing, and deploying n8n workflows.

- **instance-readiness** — Assess n8n instance readiness before provisioning. Runs health checks, inventories workflows and credentials, detects conflicts, verifies community nodes, and produces a readiness report with pass/warn/fail ratings.
Use when: "check n8n instance", "is n8n ready for provisioning", "n8n health check", "verify n8n instance", "pre-provisioning check", "audit n8n before import", "instance readiness report"

- **single-workflow-import** — Import and deploy a single workflow to an n8n instance from the official template library or community JSON source. Handles validation, auto-fix, credential stripping, and post-import verification.
Use when: "import n8n workflow", "deploy n8n template", "install n8n automation", "add workflow to n8n", "import template to n8n", "deploy workflow from JSON", "install community workflow"

- **template-discovery** — Search the official n8n template library (9,166+ templates). Use when asked to "search n8n templates", "find n8n workflow", "browse n8n template library", "n8n workflow catalog", "discover n8n automation", or need to find a template by keyword, node type, task, category, or architectural pattern.
- **troubleshoot** — Diagnose and fix n8n provisioning and import issues. This skill should be used when the user encounters "n8n import error", "template deploy failed", "workflow validation error", "provisioning troubleshoot", "n8n provision problem", "community node missing", "credential not found", "workflow won't activate", "batch deploy failed", or needs help debugging n8n workflow import and deployment issues.

- **workflow-analysis** — Analyze an n8n workflow JSON before importing — node inventory, connection topology, credential requirements, security flags, complexity scoring, and compatibility checks. Use when asked to "analyze n8n workflow", "check workflow before import", "workflow compatibility check", "review n8n template", "assess workflow complexity", or before importing any template or community workflow.

## Subagents

Defined under `.codex/agents/` as TOML files:

- **n8n-provisioner** — n8n instance provisioner and workflow sourcing specialist. Discovers existing workflows from the official template library (9,166+ templates), GitHub repositories, and community platforms, then analyzes, imports, and batch-deploys them to provision an n8n instance.

<example>
Context: User wants to set up a new n8n instance with common automation workflows
user: "I just set up a new n8n instance. Help me provision it with essential workflows for a SaaS startup."
assistant: "I'll use the n8n-provisioner agent to search for relevant templates, analyze compatibility, and batch-deploy them to your instance."
<commentary>
User needs full instance provisioning — agent searches templates by category, analyzes each, plans credentials, and batch-imports.
</commentary>
</example>

<example>
Context: User is looking for a specific workflow from the community
user: "I need an n8n workflow that syncs Notion databases with Google Sheets. Check the template library and GitHub repos."
assistant: "I'll use the n8n-provisioner agent to search across the template library and community sources for Notion-to-Sheets sync workflows."
<commentary>
User needs multi-source discovery — agent searches official templates first, then falls back to GitHub repos and community sites.
</commentary>
</example>

<example>
Context: User wants to import a specific template they found
user: "Deploy template #2947 to my n8n instance and tell me what credentials I need to set up"
assistant: "I'll use the n8n-provisioner agent to analyze the template, deploy it, and provide credential setup guidance."
<commentary>
User has a specific template ID — agent gets template details, analyzes credentials needed, deploys with auto-fix, and guides credential setup.
</commentary>
</example>


## Workflows (canonical slash commands)

Codex CLI doesn't support custom slash commands — invoke these workflows via natural language. Each entry below is a prompt template you can adapt:

### `analyze-workflow`

Analyze a workflow JSON or template before importing

Arguments: `<template-id-or-url>`

<details><summary>Prompt template</summary>

# Analyze Workflow

Analyze an n8n workflow before importing — check complexity, credentials, compatibility, and security.

## Arguments
Format: `<template-id-or-url>`
- Numeric ID: treated as an official template ID (fetched via `~~template_get`)
- URL: treated as a community source (fetched via `~~scrape`)

Parse from "$ARGUMENTS".

## Process

1. **Fetch the workflow:**
   - If numeric: use `~~template_get` with the ID
   - If URL: use `~~scrape` to read the page, extract the workflow JSON

2. **Run analysis** (follow `workflow-analysis` skill):
   - **Node inventory:** List all nodes, mark built-in vs community
   - **Topology:** Linear, branched, looped, error-handled
   - **Credentials:** List all required credential types and services
   - **Security scan:** Check for hardcoded values, open webhooks, code execution nodes
   - **Complexity score:** Simple / Medium / Complex
   - **Compatibility:** Deprecated nodes, version requirements

3. **Display analysis report** in structured format:
   ```
   Template: <name> (#<id>)
   Complexity: <Simple|Medium|Complex> (<node-count> nodes)
   Topology: <type>
   Credentials needed: <list>
   Community nodes: <list or "None">
   Security flags: <list or "None">
   Compatibility: <OK or issues>
   ```

4. **Recommend action:** Deploy, deploy with caution, or skip (with reason).

## Example Usage
```
/n8n-provision:analyze-workflow 2947
/n8n-provision:analyze-workflow https://github.com/Zie619/n8n-workflows/blob/main/workflows/1234.json
```

</details>

### `deploy-template`

Deploy an official n8n template to your instance

Arguments: `<template-id> [--name <custom-name>] [--auto-fix]`

<details><summary>Prompt template</summary>

# Deploy Template

Deploy an official n8n template to the connected n8n instance.

## Arguments
Format: `<template-id> [--name <custom-name>] [--auto-fix]`
- template-id: Numeric template ID from the n8n library (required)
- --name: Custom workflow name (default: use template name)
- --auto-fix: Enable auto-fix for minor validation issues

Parse from "$ARGUMENTS".

## Process

1. **Get template details** via `~~template_get` with the template ID at full detail level.

2. **Analyze the template:**
   - List all nodes and their types
   - Identify required credentials (services that need authentication)
   - Check for community nodes
   - Report complexity (node count, branching)

3. **Show analysis summary** and ask for confirmation before deploying.

4. **Deploy** via `~~template_deploy`:
   - Apply custom name if provided
   - Enable auto-fix if `--auto-fix` flag set
   - Workflow is imported as **inactive** by default

5. **Post-deploy report:**
   - Workflow ID and name
   - Credential setup checklist (list each credential type needed)
   - "Activate with `/n8n:activate-workflow <id>` after configuring credentials"

## Example Usage
```
/n8n-provision:deploy-template 2947
/n8n-provision:deploy-template 1234 --name "My Slack Notifier"
/n8n-provision:deploy-template 5678 --auto-fix
```

</details>

### `provision-instance`

Run a full instance provisioning session with multiple workflows

Arguments: `[--preset <startup|devops|marketing|custom>] [--dry-run]`

<details><summary>Prompt template</summary>

# Provision Instance

Batch-provision an n8n instance with a suite of workflows.

## Arguments
Format: `[--preset <startup|devops|marketing|custom>] [--dry-run]`
- --preset: Use a predefined workflow suite (default: custom — interactive selection)
  - startup: 10 essential workflows for SaaS startups
  - devops: 8 DevOps monitoring and CI/CD workflows
  - marketing: 8 marketing automation workflows
  - custom: interactive — user describes what they need, agent searches and proposes
- --dry-run: Analyze and plan without actually deploying

Parse from "$ARGUMENTS".

## Process

1. **Check instance readiness** (follow `instance-readiness` skill):
   - Health check via `~~instance_audit`
   - Existing workflow inventory via `~~workflow_list`
   - Check for naming conflicts

2. **Build workflow list:**
   - If preset: load from `batch-provisioning` skill presets (see `references/BATCH_STRATEGIES.md`)
   - If custom: ask user what they need, search via `~~template_search`, propose a list

3. **Analyze each workflow:**
   - Run `workflow-analysis` on every candidate
   - Compile credential requirements across all workflows
   - Flag any issues (community nodes, security, compatibility)

4. **Present provisioning plan:**
   - Workflow list with analysis summaries
   - Credential setup checklist (grouped by service)
   - Estimated import order (dependency-aware)
   - If `--dry-run`: stop here

5. **Execute batch import** (follow `batch-provisioning` skill):
   - Deploy one-by-one with validation between each
   - Tag with batch identifier
   - Report progress after each deployment

6. **Post-provisioning report:**
   - All deployed workflows with IDs
   - Credential setup checklist
   - Activation instructions
   - Any failures and remediation steps

## Example Usage
```
/n8n-provision:provision-instance --preset startup
/n8n-provision:provision-instance --preset devops --dry-run
/n8n-provision:provision-instance --preset custom
```

</details>

### `search-community`

Search GitHub repos and community platforms for n8n workflows

Arguments: `<query> [--source <github|community|all>]`

<details><summary>Prompt template</summary>

# Search Community

Search GitHub repositories and community platforms for n8n workflows not in the official template library.

## Arguments
Format: `<query> [--source <github|community|all>]`
- query: Search text (required)
- --source: Where to search — github (repos only), community (platforms only), all (default)

Parse from "$ARGUMENTS".

## Process

1. **Search GitHub repos** (if source is github or all):
   - Use `~~search` with queries targeting known repos: `"<query> n8n workflow site:github.com"`
   - Target high-value repos: Zie619/n8n-workflows, enescingoz/awesome-n8n-templates, Danitilahun/n8n-workflow-templates
   - See `community-source-discovery` skill and `references/GITHUB_SOURCES.md` for repo details

2. **Search community platforms** (if source is community or all):
   - Use `~~search` with queries targeting known platforms: `"<query> n8n workflow site:n8nworkflows.xyz OR site:n8nfind.net"`
   - See `references/COMMUNITY_PLATFORMS.md` for platform details

3. **Display results:**
   - Source (GitHub repo or platform name), title, URL, description
   - Quality indicators: stars (for repos), freshness
   - Note if raw JSON is directly available or needs scraping

4. **Suggest next steps:**
   - "Use `/n8n-provision:analyze-workflow <url>` to analyze a found workflow"
   - "Official templates may also have what you need — try `/n8n-provision:search-templates`"

## Example Usage
```
/n8n-provision:search-community "notion google sheets sync"
/n8n-provision:search-community "AI agent" --source github
/n8n-provision:search-community "CRM pipeline" --source all
```

</details>

### `search-templates`

Search the official n8n template library for ready-made workflows

Arguments: `<query> [--category <name>] [--nodes <nodeTypes>] [--mode <keyword|by_nodes|by_task>]`

<details><summary>Prompt template</summary>

# Search Templates

Search the n8n.io official template library (9,166+ templates) for workflows matching a query.

## Arguments
Format: `<query> [--category <name>] [--nodes <nodeTypes>] [--mode <keyword|by_nodes|by_task>]`
- query: Search text (required)
- --category: Filter by category name (e.g., marketing, devops, AI)
- --nodes: Search by node types used (comma-separated, e.g., Slack,GitHub)
- --mode: Search mode — keyword (default), by_nodes, by_task

Parse from "$ARGUMENTS".

## Process

1. **Determine search mode** based on arguments:
   - Default or `--mode keyword`: use `~~template_search` with keyword mode
   - `--nodes` provided or `--mode by_nodes`: use `~~template_search` with by_nodes mode
   - `--mode by_task`: use `~~template_search` with by_task mode (natural language)

2. **Execute search** via `~~template_search` with the query and any filters.

3. **Display results** in a table:
   - Template ID, name, node count, total views, description (truncated)
   - Sort by relevance (default from API)
   - Show total results count and current page

4. **Suggest next steps:**
   - "Use `/n8n-provision:deploy-template <id>` to deploy a template"
   - "Use `/n8n-provision:analyze-workflow <id>` to analyze before importing"

## Example Usage
```
/n8n-provision:search-templates "slack notifications"
/n8n-provision:search-templates "CRM sync" --category sales
/n8n-provision:search-templates --nodes Slack,GitHub,Webhook
/n8n-provision:search-templates "send email when form submitted" --mode by_task
```

</details>
