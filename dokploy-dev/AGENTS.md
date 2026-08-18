# dokploy-dev

> Dokploy self-hosted PaaS development plugin (aligned with Dokploy v0.29.14). Deploy applications, provision 6 database types (Postgres, MySQL, MariaDB, MongoDB, Redis, LibSQL), manage domains and Docker Compose stacks, AND debug failed deployments end-to-end — reads runtime logs of every container (including each container in a Docker Compose stack) over the API/MCP with tail/since/search, plus AI-powered log analysis (ai-analyzeLogs), Docker container introspection, Traefik diagnosis, and a guided recovery chain. Complete MCP/REST coverage: all 546 v0.29.14 operations across 50 categories indexed with params — covers forward-auth SSO domain protection, SCIM provisioning, build concurrency, and the rewritten @dokploy/cli (546 auto-generated commands incl. read-logs). Uses the official @dokploy/mcp server plus debugging-focused slash commands including /compose-logs.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/dokploy-dev

## Skills

- **ai-assist** — This skill should be used when the user wants AI-powered deployment debugging on Dokploy — wiring up an LLM provider (OpenAI, Anthropic, Gemini, Ollama, OpenRouter, etc.), summarising build logs with AI, or asking Dokploy for a next-step suggestion. Triggers: "analyze my failed deploy with AI", "ai analyze logs dokploy", "set up dokploy ai", "configure ai provider in dokploy", "why is dokploy not suggesting fixes", "dokploy ai-analyzeLogs", "dokploy ai-suggest".
- **api-reference** — This skill should be used when making direct HTTP/curl calls to the Dokploy API, looking up endpoint parameters, or building integrations that bypass the MCP server. Triggers: "dokploy API", "curl dokploy", "REST endpoint", "HTTP request to dokploy".
- **cli-recipes** — This skill should be used when running Dokploy operations from the terminal with the @dokploy/cli — authenticating, creating projects/apps, deploying, managing environment variables, provisioning databases, or reading logs via command line. Triggers: "dokploy cli", "dokploy command", "dokploy auth", "dokploy application deploy", "dokploy read-logs from terminal", "deploy dokploy from terminal".
- **debug-deploy** — This skill should be used when a Dokploy deployment fails, gets stuck, or behaves incorrectly after deploying — provides an end-to-end decision tree that locates the failed run, reads the right logs, inspects the container and Traefik state, summarises root cause with AI, and recovers safely. Triggers: "my dokploy deploy failed", "deployment stuck", "build error in dokploy", "app crashed after deploy", "diagnose failed deployment", "dokploy deploy not working", "why did my deploy fail", "recover from broken deploy".
- **examples** — This skill should be used when learning how to deploy apps, provision databases, set up Docker Compose stacks, or debug a failed deployment on Dokploy. Provides end-to-end workflow walkthroughs. Triggers: "dokploy example", "how to deploy on dokploy", "dokploy tutorial", "dokploy walkthrough", "show me how to use dokploy", "dokploy debug example".
- **mcp-patterns** — This skill should be used when deploying applications, managing projects, provisioning databases, configuring domains, working with Docker Compose, or performing any Dokploy operation via MCP tools. Triggers: "deploy app", "create project", "add domain", "provision database", "dokploy compose", "manage dokploy".
- **read-logs** — This skill should be used whenever the user wants to read, tail, stream, or search Dokploy logs — application runtime logs, Docker Compose stack logs (every container), database logs, or deployment build logs — and especially to diagnose why something failed. Triggers: "read the logs", "show me the dokploy logs", "tail the logs", "compose logs", "all containers' logs", "container logs", "why is my app crashing", "why did my deploy fail — check the logs", "grep the logs for an error", "runtime logs", "build logs". Use it instead of telling the user logs aren't available over the API — since Dokploy v0.29.0 they are.
- **setup** — This skill should be used when verifying Dokploy MCP connection, CLI installation, and API access. Use when user says "set up dokploy", "verify dokploy connection", "check dokploy", "test dokploy access", or enables the dokploy-dev plugin for the first time.
- **troubleshoot** — This skill is the symptom-to-cause lookup reference for Dokploy problems — domains, databases, Docker, Traefik, MCP connection. Use for known-symptom diagnosis. For an end-to-end failed-deploy workflow, the canonical entry point is the `debug-deploy` skill and the `/dokploy-dev:debug` command. Triggers: "dokploy 502", "domain not resolving", "database connection refused", "mcp tools not found", "dokploy api 401", "traefik dashboard".
- **add-domain** — Add a custom domain to a Dokploy application
- **analyze** — AI-summarise a failed Dokploy deployment or a crashing container via the configured ai provider
- **cleanup** — Reclaim disk space on the Dokploy server with a guided cleanup chain
- **compose-logs** — Read the logs of EVERY container in a Dokploy Docker Compose stack and highlight errors
- **create-app** — Create a new application in a Dokploy project
- **create-db** — Create and deploy a database in a Dokploy project
- **create-project** — Create a new Dokploy project
- **debug** — Debug a failed or stuck Dokploy deployment with full decision-tree analysis
- **deploy** — Deploy or redeploy a Dokploy application or Docker Compose service
- **list-apps** — List all applications and services in a Dokploy project
- **list-projects** — List all Dokploy projects
- **logs** — Read runtime or build logs for a Dokploy application, compose stack, database, or deployment
- **rollback** — Roll a Dokploy application or compose stack back to a previous version
- **status** — Check Dokploy application or deployment status

## Subagents

Codex does not install plugin subagents automatically — copy them manually before use:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

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
assistant: "I'll run the full debug-deploy workflow: locate the failed run, read the build log (or per-container runtime logs for a compose stack), inspect the container and Traefik, AI-summarise if a provider is configured, then recommend a recovery action."
<commentary>Diagnostic workflow: deployment-all → deployment-readLogs (build) or application-readLogs / per-container compose-readLogs (runtime) → docker-getContainersByAppLabel/Match → docker-getConfig → application-readTraefikConfig → ai-analyzeLogs {aiId,logs,context} (if enabled) → killBuild/redeploy/rollback. Use `/dokploy-dev:debug`, or `/dokploy-dev:compose-logs` to read every container.</commentary>
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
