# trigger-dev

> Trigger.dev dev plugin for Agents Store. Comprehensive development knowledge for building background tasks, AI agent workflows, and durable execution on self-hosted Trigger.dev v4.4.4 — SDK patterns, CLI recipes, deployment, full 33-tool MCP reference, TRQL queries and dashboards, managed prompts, dev-server control, realtime API, and troubleshooting.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/trigger-dev

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **ai-agent-patterns** — Build AI agent workflows on Trigger.dev — prompt chaining, routing, parallelization, orchestrator-workers, evaluator-optimizer, and human-in-the-loop. Use when the user asks to "build an AI agent with trigger.dev", "chain LLM calls", "parallelize AI tasks", "create an orchestrator", "add human approval to workflow", or needs patterns for LLM-powered durable execution.
- **cli-recipes** — Trigger.dev CLI commands for development, deployment, MCP setup, and project management. Use when the user asks about "trigger.dev CLI", "npx trigger.dev", "trigger.dev dev server", "trigger.dev deploy command", "trigger.dev mcp install", "trigger.dev profiles", or needs ready-to-use CLI commands.
- **config-and-build** — Configure Trigger.dev projects with trigger.config.ts, add build extensions for Prisma, Playwright, FFmpeg, Python, or customize deployment settings. Use when the user asks to "configure trigger.config.ts", "add Prisma to trigger.dev", "set up build extensions", "add FFmpeg to tasks", "configure Python in trigger.dev", or needs to customize project configuration.
- **deployment** — Deploy Trigger.dev tasks to staging, production, or preview environments. Use when the user asks to "deploy trigger.dev tasks", "set up CI/CD for trigger.dev", "deploy to production", "deploy self-hosted trigger", "manage environments", or needs deployment workflows and self-hosted infrastructure guidance.
- **examples** — Trigger.dev code examples, end-to-end workflow templates, MCP tool usage patterns, and scenario walkthroughs. Use when the user asks for "trigger.dev examples", "task templates", "MCP tool patterns", "trigger.dev workflow examples", or needs reference implementations.
- **managed-prompts** — Version and override Trigger.dev managed prompts via MCP — list, inspect, promote code versions, create dashboard overrides, hotfix, and revert. Use when the user asks about "trigger.dev prompt", "managed prompts", "prompt version", "prompt override", "hotfix prompt", "promote prompt version", "list prompts", or "revert prompt override".
- **mcp-patterns** — Trigger.dev MCP tools reference — all 33 tools, parameters, usage patterns, and common workflows as of v4.4.4. Use when the user asks about "trigger.dev MCP tools", "which trigger.dev tools are available", "how to use trigger.dev MCP", "trigger task via MCP", "list runs MCP", "profile switching", "TRQL queries from MCP", "dev server control", or "managed prompts overrides".
- **observability** — Query Trigger.dev data with TRQL, build dashboards, and inspect automatically-tracked LLM metrics. Use when the user asks about "trigger.dev query", "TRQL", "trigger.dev dashboards", "LLM metrics", "llm_metrics table", "how many runs failed this week", "cost per task", "query runs table", "span details", or "metrics table".
- **realtime** — Subscribe to Trigger.dev task runs in real-time from frontend and backend. Use when the user asks to "show task progress in UI", "stream AI responses", "use trigger.dev React hooks", "subscribe to runs", "build a progress indicator", "use useRealtimeRun", or needs live task status in a React application.
- **setup** — Set up Trigger.dev in a project, connect to a self-hosted instance, verify MCP connection, authenticate the CLI, or configure dev/staging/production environments. Use when the user asks to "set up trigger.dev", "init trigger project", "connect to self-hosted trigger", "verify trigger connection", "install trigger.dev", or "configure trigger environments".
- **task-development** — Write Trigger.dev tasks, configure retries, queues, concurrency, waits, metadata, tags, debouncing, idempotency, and use the @trigger.dev/sdk. Use when the user asks to "write a trigger.dev task", "create a background job", "add retries to task", "set up a task queue", "use triggerAndWait", "schedule a cron job", or needs SDK code patterns for Trigger.dev.
- **troubleshoot** — Diagnose and fix Trigger.dev errors, failed runs, deployment issues, and self-hosted problems. Use when the user encounters "trigger.dev not working", "task keeps failing", "deploy failed", "runs stuck", "self-hosted issues", "trigger.dev connection error", or needs to debug Trigger.dev v4.

## Subagents

Defined under `.codex/agents/` as TOML files:

- **trigger-developer** — Use this agent when the user needs help building with Trigger.dev v4 — writing background tasks, debugging failed runs, designing AI agent workflows, deploying to self-hosted instances, or working with the Trigger.dev SDK/CLI/MCP.

<example>
Context: User is writing a new background task
user: "Help me write a Trigger.dev task that processes uploaded images with FFmpeg and stores results in S3"
assistant: "I'll use the trigger-developer agent to build the image processing task."
<commentary>
Developer needs help writing a Trigger.dev task with specific integrations.
</commentary>
</example>

<example>
Context: User is debugging a failed run
user: "My trigger.dev task keeps crashing with OOM after processing large files"
assistant: "I'll use the trigger-developer agent to diagnose the crash and fix it."
<commentary>
Developer is debugging task failures — agent can analyze error patterns and suggest machine preset changes.
</commentary>
</example>

<example>
Context: User wants to design an AI agent workflow
user: "I need a pipeline that scrapes 100 URLs, processes each with AI, and aggregates results"
assistant: "I'll use the trigger-developer agent to design the orchestrator-worker pattern."
<commentary>
Developer needs architectural guidance for a complex parallelized workflow.
</commentary>
</example>


## Workflows (canonical slash commands)

Codex CLI doesn't support custom slash commands — invoke these workflows via natural language. Each entry below is a prompt template you can adapt:

### `create-task`

Create a new Trigger.dev task file

Arguments: `<task-name> [--type basic|schema|cron|agent]`

<details><summary>Prompt template</summary>

# Create Task

1. Parse arguments:
   - `task-name` (required): kebab-case task identifier
   - `--type` (optional, default: basic): basic, schema, cron, agent
2. Determine the task directory from `trigger.config.ts` `dirs` field (default: `src/trigger/`)
3. Create the task file at `<dirs>/<task-name>.ts` using the appropriate template:

**basic**: Simple task with retry config
**schema**: Task with Zod schema validation
**cron**: Scheduled task with cron expression
**agent**: AI agent task with LLM integration

4. Ensure the task is exported
5. Display: "Task created. Run `npx trigger.dev@latest dev` to register it."

</details>

### `deploy`

Deploy Trigger.dev tasks to an environment

Arguments: `[staging|production] [--profile <name>]`

<details><summary>Prompt template</summary>

# Deploy Tasks

1. Determine target environment from arguments (default: production)
2. Verify `trigger.config.ts` exists
3. Build the deploy command:
   - Base: `npx trigger.dev@latest deploy --env <environment>`
   - If `--profile` argument: add `--profile <name>`
   - If monorepo: add `--config <path>`
4. Run the deploy command
5. Report the deployment status and version
6. Suggest: "Check deployment with `list_deploys(environment='<env>', limit=1)` or in the dashboard"

</details>

### `dev`

Start the Trigger.dev dev server

Arguments: `--profile <name>`

<details><summary>Prompt template</summary>

# Start Dev Server

1. Verify `trigger.config.ts` exists in the project
2. Build the dev command:
   - Base: `npx trigger.dev@latest dev`
   - If `--profile` argument: add `--profile <name>`
   - If monorepo detected: add `--config <path-to-trigger.config.ts>`
3. Run the command
4. The dev server will watch for file changes and register tasks with the dev environment

</details>

### `init`

Initialize Trigger.dev in the current project

Arguments: `--self-hosted <url>`

<details><summary>Prompt template</summary>

# Initialize Trigger.dev

1. Check if `trigger.config.ts` already exists in the project root
2. If `--self-hosted` argument is provided, run:
   ```bash
   npx trigger.dev@latest init -a <url>
   ```
   Otherwise run:
   ```bash
   npx trigger.dev@latest init
   ```
3. Verify `trigger.config.ts` was created
4. Verify `src/trigger/` directory exists with an example task
5. Check that `@trigger.dev/sdk` is in `package.json` dependencies
6. If self-hosted, ensure `.env` has `TRIGGER_API_URL` and `TRIGGER_SECRET_KEY`
7. Display next steps: "Run `npx trigger.dev@latest dev` to start the dev server"

</details>
