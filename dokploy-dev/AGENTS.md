# dokploy-dev

> Dokploy self-hosted PaaS development plugin (aligned with Dokploy v0.29.x). Deploy applications, provision 6 database types (Postgres, MySQL, MariaDB, MongoDB, Redis, LibSQL), manage domains and Docker Compose stacks, AND debug failed deployments end-to-end with AI-powered log analysis (ai-analyzeLogs), Docker container introspection, Traefik diagnosis, and a guided recovery chain. Uses the official @dokploy/mcp server (500+ tools across 49 categories) plus 5 debugging-focused slash commands.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/dokploy-dev

## MCP servers

Add to `~/.codex/config.toml`:

```toml
[mcp_servers.dokploy]
command = "npx"
args = ["-y","@dokploy/mcp"]

[mcp_servers.dokploy.env]
"DOKPLOY_URL" = "${DOKPLOY_URL}"
"DOKPLOY_API_KEY" = "${DOKPLOY_API_KEY}"

```

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **ai-assist** — This skill should be used when the user wants AI-powered deployment debugging on Dokploy — wiring up an LLM provider (OpenAI, Anthropic, Gemini, Ollama, OpenRouter, etc.), summarising build logs with AI, or asking Dokploy for a next-step suggestion. Triggers: "analyze my failed deploy with AI", "ai analyze logs dokploy", "set up dokploy ai", "configure ai provider in dokploy", "why is dokploy not suggesting fixes", "dokploy ai-analyzeLogs", "dokploy ai-suggest".
- **api-reference** — This skill should be used when making direct HTTP/curl calls to the Dokploy API, looking up endpoint parameters, or building integrations that bypass the MCP server. Triggers: "dokploy API", "curl dokploy", "REST endpoint", "HTTP request to dokploy".
- **cli-recipes** — This skill should be used when running Dokploy operations from the terminal, deploying via CLI, managing environment variables with env push/pull, or provisioning databases via command line. Triggers: "dokploy cli", "dokploy command", "env push", "env pull", "dokploy deploy cli".
- **debug-deploy** — This skill should be used when a Dokploy deployment fails, gets stuck, or behaves incorrectly after deploying — provides an end-to-end decision tree that locates the failed run, reads the right logs, inspects the container and Traefik state, summarises root cause with AI, and recovers safely. Triggers: "my dokploy deploy failed", "deployment stuck", "build error in dokploy", "app crashed after deploy", "diagnose failed deployment", "dokploy deploy not working", "why did my deploy fail", "recover from broken deploy".
- **examples** — This skill should be used when learning how to deploy apps, provision databases, set up Docker Compose stacks, or debug a failed deployment on Dokploy. Provides end-to-end workflow walkthroughs. Triggers: "dokploy example", "how to deploy on dokploy", "dokploy tutorial", "dokploy walkthrough", "show me how to use dokploy", "dokploy debug example".
- **mcp-patterns** — This skill should be used when deploying applications, managing projects, provisioning databases, configuring domains, working with Docker Compose, or performing any Dokploy operation via MCP tools. Triggers: "deploy app", "create project", "add domain", "provision database", "dokploy compose", "manage dokploy".
- **setup** — This skill should be used when verifying Dokploy MCP connection, CLI installation, and API access. Use when user says "set up dokploy", "verify dokploy connection", "check dokploy", "test dokploy access", or enables the dokploy-dev plugin for the first time.
- **troubleshoot** — This skill is the symptom-to-cause lookup reference for Dokploy problems — domains, databases, Docker, Traefik, MCP connection. Use for known-symptom diagnosis. For an end-to-end failed-deploy workflow, the canonical entry point is the `debug-deploy` skill and the `/dokploy-dev:debug` command. Triggers: "dokploy 502", "domain not resolving", "database connection refused", "mcp tools not found", "dokploy api 401", "traefik dashboard".

## Subagents

Defined under `.codex/agents/` as TOML files:

- **dokploy-assistant** — Dokploy development assistant for deploying applications, managing projects, provisioning databases, and configuring domains on a self-hosted Dokploy PaaS instance.

<example>
Context: User wants to deploy a web application from GitHub
user: "Deploy my Next.js app from github.com/myorg/myapp to Dokploy with a custom domain"
assistant: "I'll create a project, set up the application with your GitHub repo, configure the domain, and deploy it."
<commentary>Full deployment workflow: create project, create app, connect git, set build type, add domain, deploy.</commentary>
</example>

<example>
Context: User needs to debug a failed deployment
user: "My deployment is failing with a build error, can you check what's wrong?"
assistant: "I'll run the full debug-deploy workflow: locate the failed run, read the build log, inspect the container and Traefik, AI-summarise if a provider is configured, then recommend a recovery action."
<commentary>Diagnostic workflow: deployment-all → application-readLogs → docker-getContainersByAppLabel → docker-getConfig → application-readTraefikConfig → ai-analyzeLogs (if enabled) → killBuild/redeploy/rollback. Use `/dokploy-dev:debug` to chain it.</commentary>
</example>

<example>
Context: User's deploy "succeeded" but the live site is unchanged
user: "I deployed but nothing changed on production"
assistant: "Likely a compose-mode mismatch — the standalone application deployed but production runs from a compose service. I'll check the project layout and deploy the compose resource instead."
<commentary>Common silent-failure pattern. Inspect project-one for both an application AND a compose service; if compose exists, deploy compose instead of the standalone app.</commentary>
</example>

<example>
Context: User wants to set up AI-powered log analysis
user: "Can Dokploy summarise build errors for me using AI?"
assistant: "Yes — I'll wire up an AI provider via the ai-* router, test the connection, and from then on `/dokploy-dev:analyze` runs ai-analyzeLogs against any failed deployment."
<commentary>v0.29 AI router: ai-create → ai-testConnection → ai-analyzeLogs. Provider-agnostic, OpenAI-compatible. See the `ai-assist` skill.</commentary>
</example>

<example>
Context: User wants to provision a database with backups
user: "Set up a PostgreSQL database for my project with daily backups"
assistant: "I'll create a PostgreSQL instance, deploy it, configure external access, and set up automated backups."
<commentary>Database provisioning workflow: create instance, deploy, configure external port, set up automated backup schedule.</commentary>
</example>


## Workflows (canonical slash commands)

Codex CLI doesn't support custom slash commands — invoke these workflows via natural language. Each entry below is a prompt template you can adapt:

### `add-domain`

Add a custom domain to a Dokploy application

Arguments: `<domain> --app <app-name-or-id> [--port <port>] [--https]`

<details><summary>Prompt template</summary>

# Add Domain

Add a custom domain to a Dokploy application with optional HTTPS.

## Arguments
Format: `<domain> --app <app-name-or-id> [--port <port>] [--https]`
- domain: Domain name, e.g. app.example.com (required)
- --app: Application name or ID (required)
- --port: Application port (default: 3000)
- --https: Enable HTTPS with Let's Encrypt certificate (optional flag)

Parse from "$ARGUMENTS".

## Process

1. **Resolve application** — if --app is a name, resolve via `project-all` and `project-one`.

2. **Create domain** using MCP tool `domain-create` with:
   - `host`: the domain name
   - `applicationId`: resolved application ID
   - `port`: specified port or 3000
   - `path`: `/`
   - `https`: true if --https flag provided

3. **Validate domain** using MCP tool `domain-validateDomain` to check DNS resolution.

4. **Display result:**
   Show domain, port, HTTPS status, and validation result. If DNS not resolving, remind user to add an A record pointing to the server IP.

## Example Usage
```
/dokploy-dev:add-domain app.example.com --app web-frontend --https
/dokploy-dev:add-domain api.example.com --app api-server --port 8000 --https
/dokploy-dev:add-domain staging.example.com --app web-frontend --port 3000
```

</details>

### `analyze`

AI-summarise a failed Dokploy deployment via the configured ai provider

Arguments: `deployment-id|app-name-or-id`

<details><summary>Prompt template</summary>

# AI-Analyse Deployment

One-shot wrapper around `ai-analyzeLogs`. Locates a failed deployment, runs the configured AI provider against its build log, and reports root cause + suggested fix.

## Arguments

Format: `[deployment-id|app-name-or-id]` (optional)

- If a deployment ID is passed, analyse that specific deployment.
- If an application/compose name or ID is passed, find its most recent failed deployment.
- If **no argument**, call `deployment-allCentralized`, filter to the most recent `status: error` rows, and ask the user which to analyse.

Parse from "$ARGUMENTS".

## Process

1. **Verify AI is configured** — `mcp__dokploy__ai-getEnabledProviders`. If empty:
   - Tell the user no AI provider is configured.
   - Offer the manual debug path: "Run `/dokploy-dev:debug <id>` instead."
   - Optionally point them at the `ai-assist` skill to wire one up.
   - Stop here.

2. **Resolve the deployment ID:**
   - If a deployment ID is passed, use it directly.
   - If a resource name/ID is passed, call `deployment-all { applicationId }` (or `composeId`), filter to `status: error`, take the most recent.

3. **Run the analysis:**

   ```
   mcp__dokploy__ai-analyzeLogs
     → { deploymentId }
   ```

4. **Present the result** in this format:

   ```
   ## AI Analysis: <appName / composeName>

   **Deployment:** <deploymentId>
   **Started:** <timestamp>
   **Status:** error

   ### Root cause (per <providerName>/<model>)
   <ai-analyzeLogs.rootCause>

   ### Summary
   <ai-analyzeLogs.summary>

   ### Suggested fix
   <ai-analyzeLogs.suggestedFix>

   ### Verify before applying
   - Compare against the actual build log: `<logPath>`
   - Run `/dokploy-dev:logs <id>` to see the raw output
   - LLMs occasionally invent fixes for symptoms they don't fully understand
   ```

5. **Offer follow-up:**
   - "Apply this fix? I can update env / build type / Dockerfile and redeploy."
   - "Want a deeper analysis? Run `/dokploy-dev:debug <id>` for the full decision tree."

## Example Usage

```
/dokploy-dev:analyze
/dokploy-dev:analyze web-frontend
/dokploy-dev:analyze deploy_abc123def456
```

</details>

### `cleanup`

Reclaim disk space on the Dokploy server with a guided cleanup chain

Arguments: `--dry-run`

<details><summary>Prompt template</summary>

# Dokploy Server Cleanup

Guided cleanup chain to reclaim disk space. Always reports current usage first, then walks through each cleanup operation with explicit user confirmation.

Use this when builds start failing with `no space left on device`, deploys silently time out, or `settings-getDockerDiskUsage` reports >90% utilisation.

## Arguments

Format: `[--dry-run]` (optional)

- `--dry-run` — show what would be cleaned without actually executing any destructive operation.

Parse from "$ARGUMENTS".

## Process

1. **Report current state:**

   ```
   mcp__dokploy__settings-getDockerDiskUsage
   ```

   Show:
   - Total / used / available disk
   - Per-category sizes: Images, Containers, Volumes, Build cache
   - Top 5 largest images / volumes (if returned)

2. **Walk through cleanup operations** in this order. Confirm each with the user (unless `--dry-run`):

   | Step | Tool | What it does | Risk |
   |------|------|--------------|------|
   | a | `mcp__dokploy__settings-cleanDockerBuilder` | Clears the Docker BuildKit cache | None — only cache |
   | b | `mcp__dokploy__settings-cleanStoppedContainers` | Removes containers in `exited` state | None — already stopped |
   | c | `mcp__dokploy__settings-cleanUnusedImages` | Removes images not currently used by any container | Low — images can be rebuilt |
   | d | `mcp__dokploy__settings-cleanUnusedVolumes` | Removes volumes not attached to any container | **Medium — destroys data**. Confirm explicitly; orphan volumes can still contain DB files |
   | e | `mcp__dokploy__settings-cleanDockerPrune` | Equivalent to `docker system prune` | Low — combination of a-c |
   | f | `mcp__dokploy__settings-cleanMonitoring` | Removes monitoring data | None |
   | g | `mcp__dokploy__settings-cleanRedis` | Flushes Dokploy's internal Redis cache | None — Dokploy will repopulate |

   Skip steps `d` and `g` unless the user explicitly opts in.

3. **Report final state:**
   - Re-run `settings-getDockerDiskUsage` and show the delta in plain language ("Reclaimed 12.4 GB").

4. **Configure log cleanup automation (optional):**
   - `mcp__dokploy__settings-getLogCleanupStatus` — show current schedule.
   - Offer to enable / tune via `settings-updateLogCleanup` if disk pressure was caused by log accumulation.

## When to stop

If `cleanDockerBuilder` + `cleanUnusedImages` reclaim less than 5% disk, the bottleneck isn't Docker. Check:

- Big application volumes (databases that grew unbounded).
- `/etc/dokploy/logs/` accumulation — `getLogCleanupStatus` will show if rotation is off.
- The host filesystem outside Docker (`/var/log`, large user files).

In that case, escalate: ssh to the server and run `du -sh /var/lib/docker /etc/dokploy /var/log`.

## Example Usage

```
/dokploy-dev:cleanup
/dokploy-dev:cleanup --dry-run
```

</details>

### `create-app`

Create a new application in a Dokploy project

Arguments: `<name> --project <project> [--build <nixpacks|dockerfile|static>]`

<details><summary>Prompt template</summary>

# Create Application

Create a new application in an existing Dokploy project.

## Arguments
Format: `<name> --project <project> [--build <nixpacks|dockerfile|static>]`
- name: Application name (required)
- --project: Project name or ID (required)
- --build: Build type — nixpacks, dockerfile, heroku, paketo, railpack, or static (default: nixpacks)

Parse from "$ARGUMENTS".

## Process

1. **Resolve project:** If --project is a name, call `project-all` and find matching project.

2. **Create application** using MCP tool `application-create` with:
   - `name`: the app name
   - `appName`: kebab-case version of name (for Docker container naming)
   - `projectId`: resolved project ID

3. **Set build type** using MCP tool `application-saveBuildType` if --build is specified.

4. **Display result:**
   Show application ID, name, project, build type. Suggest next steps: connect git repo, set env vars, add domain, deploy.

## Example Usage
```
/dokploy-dev:create-app "web-frontend" --project my-saas
/dokploy-dev:create-app "api-server" --project my-saas --build dockerfile
/dokploy-dev:create-app "landing-page" --project marketing --build static
```

</details>

### `create-db`

Create and deploy a database in a Dokploy project

Arguments: `<name> --project <project> --type <postgres|mysql|mariadb|mongo|redis> [--password <pass>]`

<details><summary>Prompt template</summary>

# Create Database

Create and deploy a database instance in a Dokploy project.

## Arguments
Format: `<name> --project <project> --type <postgres|mysql|mariadb|mongo|redis> [--password <pass>]`
- name: Database name (required)
- --project: Project name or ID (required)
- --type: Database type — postgres, mysql, mariadb, mongo, redis (required)
- --password: Database password (optional, auto-generated if omitted)

Parse from "$ARGUMENTS".

## Process

1. **Resolve project** (same as create-app command).

2. **Create database** using the type-specific MCP tool:
   - postgres: `postgres-create`
   - mysql: `mysql-create`
   - mariadb: `mariadb-create`
   - mongo: `mongo-create`
   - redis: `redis-create`

   Pass: name, projectId, databasePassword (if provided).

3. **Deploy database** using `{type}-deploy` with the created database ID.

4. **Display result:**
   Show database ID, name, type, status. Include internal connection details (host, port, credentials) and suggest using `{type}-saveExternalPort` if external access is needed.

## Example Usage
```
/dokploy-dev:create-db "main-db" --project my-saas --type postgres
/dokploy-dev:create-db "cache" --project my-saas --type redis
/dokploy-dev:create-db "analytics" --project data --type mongo --password "secure123"
```

</details>

### `create-project`

Create a new Dokploy project

Arguments: `<name> [--description <text>]`

<details><summary>Prompt template</summary>

# Create Project

Create a new project on the Dokploy instance.

## Arguments
Format: `<name> [--description <text>]`
- name: Project name (required)
- --description: Project description (optional)

Parse from "$ARGUMENTS".

## Process

1. **Create project** using MCP tool `project-create` with the provided name and description.

2. **Display result:**
   Show project ID, name, and description. Suggest next steps: create an application or database.

## Example Usage
```
/dokploy-dev:create-project "my-saas"
/dokploy-dev:create-project "staging-env" --description "Staging environment for testing"
```

</details>

### `debug`

Debug a failed or stuck Dokploy deployment with full decision-tree analysis

Arguments: `app-name-or-id|compose-id`

<details><summary>Prompt template</summary>

# Debug Failed Deployment

Run the full `debug-deploy` workflow against an application or compose stack — locate the failed run, read build logs, inspect container state, check Traefik, optionally AI-summarise, and recover.

## Arguments

Format: `[app-name-or-id|compose-id]` (optional)

- If a UUID is passed, treat it as either an `applicationId` or `composeId` and disambiguate by calling `application-one` first, falling back to `compose-one`.
- If a name is passed, call `project-all` to resolve.
- If **no argument** is passed, call `mcp__dokploy__deployment-allCentralized` and `mcp__dokploy__deployment-queueList`, list the recent `error` / stuck deployments, and ask the user which to investigate.

Parse from "$ARGUMENTS".

## Process

Read `${CLAUDE_PLUGIN_ROOT}/skills/debug-deploy/SKILL.md` and follow every step in order. Do not skip.

Key checkpoints:

1. **Step 0 — Platform health.** Run `settings-health`, `checkInfrastructureHealth`, `getDockerDiskUsage`. If any fail, fix server before deploy issue.
2. **Step 1 — Locate the failed run.** Use `deployment-all` filtered by the resource ID. Save `deploymentId` and `logPath`.
3. **Step 2 — Read the build log.** Use `application-readLogs` / `compose-readLogs` for metadata; for the actual log content, use Beszel (`beszel-getContainerLogs`) or ask the user to `tail` the log file on the server (Dokploy does not expose runtime stdout via REST — issue #3719). Match against the build-failure pattern table.
4. **Step 3 — Container introspection.** `docker-getContainersByAppLabel { appName }` for state/health. `docker-getConfig` for env/command/mounts. Use `docker-restartContainer` / `killContainer` if wedged.
5. **Step 4 — Request path.** Only if container is running but HTTP requests fail. `application-readTraefikConfig`, check port, network, listen address.
6. **Step 5 — Recovery.** Use the smallest action that unblocks: `killBuild` / `cancelDeployment` / `cleanQueues` / `dropDeployment` / `rollback-rollback`. Confirm destructive ops with the user.
7. **Step 6 — AI summary.** If `ai-getEnabledProviders` is non-empty, call `ai-analyzeLogs { deploymentId }` and present the result. Otherwise note that AI is not configured and continue manually.
8. **Step 7 — Verify the fix.** After applying a fix, `application-redeploy` (or `compose-redeploy`), poll `deployment-all` until `status: done`, validate domains, hit the endpoint with curl.

## Output format

Report findings in this structure:

```
## Diagnosis: <appName / composeName>

**Failed deployment:** <deploymentId> (status: error, started: <timestamp>)
**Build log:** <logPath>

### Root cause
<one-paragraph plain-language explanation>

### Evidence
- <log excerpt 1>
- <container state observation>
- <traefik / domain observation if relevant>

### AI summary (if available)
<ai-analyzeLogs output, condensed>

### Recommended fix
<concrete action, with the exact MCP call(s) needed>

### Recovery commands
<list of MCP calls to apply the fix and redeploy>
```

After applying a fix, re-verify before declaring the issue resolved.

## Example Usage

```
/dokploy-dev:debug
/dokploy-dev:debug web-frontend
/dokploy-dev:debug abc123-def456
```

</details>

### `deploy`

Deploy or redeploy a Dokploy application or Docker Compose service

Arguments: `<app-name-or-id> [--project <project>]`

<details><summary>Prompt template</summary>

# Deploy Application

Trigger a deployment for an existing Dokploy application.

## Arguments
Format: `<app-name-or-id> [--project <project>]`
- app-name-or-id: Application name or ID (required)
- --project: Project name or ID (helps resolve app by name)

Parse from "$ARGUMENTS".

## Process

1. **Resolve application:**
   - If argument is a UUID, use it directly.
   - If a name, call `project-all` to list projects, then `project-one` on the matching project (or --project) to find the application by name.

2. **Detect Docker Compose build mode:** Call `project-all` (or `project-one`) and check if the project has a compose service alongside the application. If a compose service exists, the production site likely runs from the compose service — NOT the standalone application. In this case:
   - Warn the user: "This project has a Docker Compose service (`<compose-name>`). The site likely runs from compose, not the standalone app. Deploy the compose service instead?"
   - If user confirms, deploy the **compose** service using `compose-deploy` MCP tool (or REST API fallback: `POST /api/compose.deploy` with `{"composeId": "<id>"}` and `x-api-key` header).
   - If `compose-deploy` MCP tool is not available, use the REST API fallback automatically.
   - Do NOT deploy only the standalone application when a compose service exists — this silently succeeds but leaves the site unchanged.

3. **Check current state** using MCP tool `application-one` (or `compose-one` for compose) with the ID. Report current status and last deployment.

4. **Pre-deploy checks (application mode only — skip for compose):**
   - **Environment variables:** Check if `env` is set on the application. If empty, read the project's local `.env.local` or `.env` file and set runtime env vars via `application-saveEnvironment`. Separate build-time vars (e.g. `NEXT_PUBLIC_*`) from runtime-only vars — build-time vars must also go into `buildArgs`.
   - **Build type:** Check `buildType`. If the project has a `Dockerfile`, ask the user which build type to use (`dockerfile` or `nixpacks`). Default recommendation: `dockerfile` when a Dockerfile exists. Set via `application-saveBuildType` with all required fields (`applicationId`, `buildType`, `dockerfile`, `dockerContextPath`, `dockerBuildStage`, `herokuVersion`, `railpackVersion`).

5. **Deploy:**
   - **Compose mode:** Use `compose-deploy` MCP tool or REST API fallback.
   - **Application mode:** Use `application-deploy` MCP tool.

6. **Monitor until completion:**
   - Poll `deployment.all?applicationId=<id>` every 30-60 seconds to check latest deployment status.
   - If status is `done` — report success and verify the app is reachable (check health endpoint or domain).
   - If status is `error` — read deployment logs via Beszel container logs or Dokploy log endpoints, diagnose the issue, fix it (update env vars, build type, Dockerfile, etc.), and redeploy. Repeat until deployment succeeds.
   - Show the user build logs and errors transparently.

## Example Usage
```
/dokploy-dev:deploy web-frontend --project my-saas
/dokploy-dev:deploy abc123-def456
```

</details>

### `list-apps`

List all applications and services in a Dokploy project

Arguments: `<project-name-or-id>`

<details><summary>Prompt template</summary>

# List Applications

List all applications, databases, and compose services in a Dokploy project.

## Arguments
Format: `<project-name-or-id>`
- project-name-or-id: Project name or ID (required). If a name is given, resolve it to ID via `project-all` first.

Parse from "$ARGUMENTS".

## Process

1. **Resolve project:** If argument looks like a name (not a UUID), call `project-all` and find the matching project by name.

2. **Get project details** using MCP tool `project-one` with the projectId.

3. **Display as table:**
   Show all services grouped by type:
   - **Applications:** ID, name, status, build type, git repo
   - **Databases:** ID, name, type (postgres/mysql/mariadb/mongo/redis), status
   - **Compose:** ID, name, status

## Example Usage
```
/dokploy-dev:list-apps my-saas
/dokploy-dev:list-apps abc123-def456
```

</details>

### `list-projects`

List all Dokploy projects

<details><summary>Prompt template</summary>

# List Projects

List all projects on the Dokploy instance.

## Process

1. **Fetch projects** using MCP tool `project-all`.

2. **Display as table:**
   Show project ID, name, description, and number of services (applications, databases, compose).

## Example Usage
```
/dokploy-dev:list-projects
```

</details>

### `logs`

Read logs for a Dokploy application, compose stack, database, or deployment

Arguments: `<resource-name-or-id> [--type <app|compose|db|deployment>] [--tail <N>]`

<details><summary>Prompt template</summary>

# Read Dokploy Logs

Unified log reader. Resolves the resource type, picks the right MCP tool, and falls back to file-path / Beszel for runtime stdout (which Dokploy does not yet expose via REST — see [issue #3719](https://github.com/Dokploy/dokploy/issues/3719)).

## Arguments

Format: `<resource-name-or-id> [--type <app|compose|db|deployment>] [--tail <N>]`

- `resource-name-or-id` — required. Name or UUID of the app / compose / database / deployment.
- `--type` — optional. Force a specific resource type. If omitted, auto-detect by trying `application-one` → `compose-one` → `<db>-one` → `deployment-all` in that order.
- `--tail` — optional. Number of recent lines to return (default 200). Honored only for the file/Beszel fallback paths.

Parse from "$ARGUMENTS".

## Process

1. **Resolve the resource:**
   - If `--type` is set, jump straight to that branch.
   - Else, try `application-one` → on miss, `compose-one` → on miss, each `{type}-one` for `postgres/mysql/mariadb/mongo/redis/libsql` → on miss, `deployment-allCentralized` and search by id.

2. **Pick the right tool:**

   | Resource type | MCP tool | Returns |
   |---|---|---|
   | Application | `application-readLogs { applicationId }` | Build log metadata + `logPath` |
   | Compose | `compose-readLogs { composeId }` | Build log metadata + `logPath` |
   | Database (any of 6) | `{type}-readLogs { {type}Id }` | DB container log metadata |
   | Deployment | `deployment-all` filtered to find this run | Includes `logPath` for the build log |

3. **Stream the actual log content:**
   - `application-readLogs` / `compose-readLogs` return the build log **path** (and sometimes a snapshot), not a live tail. For real content:
     - **Preferred:** if Beszel is installed, call `beszel-getContainerLogs` against the Dokploy container — the log files at `/etc/dokploy/logs/<appName>/*.log` are visible inside it.
     - **Fallback:** print the `logPath` and tell the user to `sudo tail -n <N> <logPath>` on the server.
   - For *database* logs and *runtime* container stdout, `{type}-readLogs` works directly via MCP.

4. **Format the output** so the user can immediately spot the relevant section:
   - Show last `--tail` lines.
   - Highlight ERROR / FATAL / panic / exited / unauthorized lines.
   - If the deployment has `status: error`, surface the last 30 lines first.

5. **Suggest next step** when an error pattern is recognised — usually `/dokploy-dev:debug <id>` for a full diagnosis, or `/dokploy-dev:analyze <deploymentId>` to AI-summarise.

## Example Usage

```
/dokploy-dev:logs web-frontend
/dokploy-dev:logs my-stack --type compose
/dokploy-dev:logs pg-main --type db --tail 500
/dokploy-dev:logs deploy_abc123 --type deployment
```

</details>

### `rollback`

Roll a Dokploy application or compose stack back to a previous version

Arguments: `<app-name-or-id|compose-id>`

<details><summary>Prompt template</summary>

# Rollback Deployment

Guided rollback to a previously-successful deployment. Lists available rollback points, asks the user to pick one, then calls `rollback-rollback`.

## Arguments

Format: `<app-name-or-id|compose-id>` (required)

Parse from "$ARGUMENTS".

## Process

1. **Resolve the resource:**
   - Try `application-one { applicationId }` first.
   - Fall back to `compose-one { composeId }`.
   - If a name was passed, use `project-all` to find the matching project and resource.

2. **List available rollback points:**
   - From the resource record, look for the `rollbacks` array (each entry has `rollbackId`, `version`, `deploymentId`, `createdAt`, `description`).
   - If there are no rollbacks, tell the user — Dokploy only creates rollback points on successful deploys. Stop here.

3. **Present options** as a numbered list:

   ```
   Available rollback points for <appName>:

   1. v3 — 2026-05-20 14:22 UTC (current: v4)
      Deployment: deploy_xyz789
   2. v2 — 2026-05-19 11:08 UTC
   3. v1 — 2026-05-18 09:30 UTC
   ```

4. **Confirm with the user:**
   - "Which version do you want to roll back to? This will stop the current container and start the rolled-back image."
   - Use AskUserQuestion for the choice if running in an interactive context.

5. **Execute:**

   ```
   mcp__dokploy__rollback-rollback
     → { rollbackId: "<chosen rollbackId>" }
   ```

6. **Verify:**
   - Poll `application-one` (or `compose-one`) until `applicationStatus: running`.
   - `docker-getContainersByAppLabel { appName }` to confirm the image tag matches the rolled-back version.
   - `domain-validateDomain` if domains are attached.
   - Curl the endpoint to confirm responsive.

7. **Tidy up (optional):**
   - Offer to delete the failed deployments that triggered the rollback via `deployment-removeDeployment` or `application-dropDeployment`. Confirm before doing so — audit trail matters.

## Notes

- Rollbacks are **image-level**, not source-level. The rolled-back container uses the previously-built image; no rebuild happens.
- Environment variables are **not** rolled back automatically. If the env changed alongside the bad deploy, ask the user whether to also restore prior env values (Dokploy keeps env history in deployment records).
- Rollback does not affect attached databases or persistent volumes.

## Example Usage

```
/dokploy-dev:rollback web-frontend
/dokploy-dev:rollback abc123-def456
```

</details>

### `status`

Check Dokploy application or deployment status

Arguments: `<app-name-or-id> [--project <project>]`

<details><summary>Prompt template</summary>

# Check Status

Check the current status of a Dokploy application and its recent deployments.

## Arguments
Format: `<app-name-or-id> [--project <project>]`
- app-name-or-id: Application name or ID (required)
- --project: Project name or ID (helps resolve app by name)

Parse from "$ARGUMENTS".

## Process

1. **Resolve application** (same as deploy command).

2. **Detect Docker Compose build mode:** Call `project-all` (or `project-one`) and check if the project has a compose service alongside the application. If a compose service exists, warn the user: "This project also has a Docker Compose service (`<compose-name>`) — the production site may run from compose, not the standalone app. Showing status of both."

3. **Get application details** using MCP tool `application-one`. Show:
   - Application status (running/stopped/error)
   - Build type
   - Git repository and branch (if connected)
   - Environment variables count
   - Domains attached

4. **Get compose details** (if compose service exists) using MCP tool `compose-one`. Show:
   - Compose status
   - Source type (GitHub/Git)
   - Domains attached
   - Flag if auto-deploy is connected to the application but NOT to compose — this is a common misconfiguration

5. **Get recent deployments** using MCP tool `deployment-all` filtered by application. Show last 5 deployments:
   - Deployment ID, status, trigger type, start time, duration

6. **Get monitoring data** using MCP tool `application-readAppMonitoring` if available. Show CPU/memory usage.

## Example Usage
```
/dokploy-dev:status web-frontend --project my-saas
/dokploy-dev:status abc123-def456
```

</details>
