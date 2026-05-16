# dokploy-dev

> Dokploy self-hosted PaaS development plugin. Deploy applications, provision databases (Postgres, MySQL, MariaDB, MongoDB, Redis, LibSQL), manage domains, Docker Compose stacks, backups, and server operations via the official @dokploy/mcp server (500+ tools across 49 categories), 463 REST API endpoints, and CLI.

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

- **api-reference** — This skill should be used when making direct HTTP/curl calls to the Dokploy API, looking up endpoint parameters, or building integrations that bypass the MCP server. Triggers: "dokploy API", "curl dokploy", "REST endpoint", "HTTP request to dokploy".
- **cli-recipes** — This skill should be used when running Dokploy operations from the terminal, deploying via CLI, managing environment variables with env push/pull, or provisioning databases via command line. Triggers: "dokploy cli", "dokploy command", "env push", "env pull", "dokploy deploy cli".
- **examples** — This skill should be used when learning how to deploy apps, provision databases, or set up Docker Compose stacks on Dokploy. Provides end-to-end workflow walkthroughs. Triggers: "dokploy example", "how to deploy on dokploy", "dokploy tutorial", "dokploy walkthrough", "show me how to use dokploy".
- **mcp-patterns** — This skill should be used when deploying applications, managing projects, provisioning databases, configuring domains, working with Docker Compose, or performing any Dokploy operation via MCP tools. Triggers: "deploy app", "create project", "add domain", "provision database", "dokploy compose", "manage dokploy".
- **setup** — This skill should be used when verifying Dokploy MCP connection, CLI installation, and API access. Use when user says "set up dokploy", "verify dokploy connection", "check dokploy", "test dokploy access", or enables the dokploy-dev plugin for the first time.
- **troubleshoot** — This skill should be used when diagnosing Dokploy deployment failures, domain issues, database connection problems, or Docker/Traefik errors. Use when a deployment fails, domain is not resolving, database cannot connect, app returns 502, or MCP tools return errors. Triggers: "dokploy error", "deployment failed", "domain not working", "502 bad gateway", "database connection refused", "dokploy debug".

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
assistant: "I'll check the deployment logs and application configuration to diagnose the issue."
<commentary>Diagnostic workflow: check deployment logs, verify environment, check build type and app configuration.</commentary>
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
