# stack-composable-stack-v1

> Composable Stack v1 architecture plugin. How PostgreSQL (direct MCP + PostgREST API), NocoDB, n8n, Trigger.dev, and NocoBase (prod + dev sandbox) fit together for data-driven applications with low-code interfaces: layer roles, data-access selection, integration patterns between services, and project bootstrap. Tool knowledge comes from its dependencies.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/stack-composable-stack-v1

## Skills

- **background-job** — This skill should be used when the user wants to "create a background job", "run async task", "process data in background", "schedule recurring task", "set up a queue", "choose between n8n and Trigger.dev", or needs to decide how background processing is split between Trigger.dev and n8n in the Composable Stack.
- **data-access-selection** — This skill should be used when the user needs to choose how to read or write data in the Composable Stack — "NocoDB MCP or PostgreSQL MCP", "PostgREST or MCP", "which data access should I use", "how should n8n read the database", "how should a Trigger.dev task call PostgreSQL", "complex SQL or simple CRUD", "batch data operation", or is picking between the stack's data-access paths.
- **full-feature** — This skill should be used when the user wants to "build a complete feature", "create end-to-end functionality", "implement a full feature across all layers", "build feature with data model and automation", or needs a step-by-step recipe for building features that span the Data, Logic, and Interface layers of the Composable Stack.
- **init-project** — This skill should be used when the user asks to "set up composable stack", "initialize project", "configure environment", "connect MCP services", or needs to set up all MCP connections and environment variables for the Composable Stack v1.
- **nocobase-to-n8n** — This skill should be used when the user wants to "trigger n8n from NocoBase", "connect NocoBase UI to n8n", "automate NocoBase actions with n8n", "create NocoBase workflow that calls n8n", or needs to integrate NocoBase interface events with n8n workflow automation.
- **nocodb-to-n8n** — This skill should be used when the user wants to "trigger n8n workflow from NocoDB", "connect NocoDB data to n8n", "create webhook from NocoDB to n8n", "automate NocoDB with n8n", or needs to integrate NocoDB data events with n8n workflow automation.
- **nocodb-to-trigger** — This skill should be used when the user wants to "trigger background task from NocoDB", "connect NocoDB to Trigger.dev", "process NocoDB data with Trigger.dev", "run background job on NocoDB change", or needs to integrate NocoDB data events with Trigger.dev background tasks.

## Subagents

Codex does not install plugin subagents automatically — copy them manually before use:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- **stack-orchestrator** — Use this agent when the user needs help coordinating work across PostgreSQL, NocoDB, n8n, Trigger.dev, and NocoBase — building features that span multiple services, debugging cross-service issues, or planning multi-layer operations in the Composable Stack.

<example>
Context: User is building a feature that spans Data and Logic layers
user: "Build an automated order processing flow that creates a NocoDB record and triggers an n8n payment workflow"
assistant: "I'll use the stack-orchestrator agent to coordinate the implementation across NocoDB and n8n."
<commentary>Feature spans Data and Logic layers — orchestrator plans the data model, webhook, and workflow.</commentary>
</example>

<example>
Context: User is debugging a cross-service integration issue
user: "My NocoDB webhook isn't triggering the n8n workflow — records are created but nothing happens"
assistant: "I'll use the stack-orchestrator agent to diagnose the NocoDB-to-n8n connection."
<commentary>Cross-service debugging requires checking both NocoDB webhook config and n8n workflow trigger.</commentary>
</example>

<example>
Context: User wants to plan a full-stack feature
user: "I need a user onboarding system with a NocoBase form, NocoDB data storage, n8n email automation, and Trigger.dev background processing"
assistant: "I'll use the stack-orchestrator agent to design the full-stack implementation plan."
<commentary>Full-stack feature spanning all 5 services — orchestrator coordinates the design across all layers.</commentary>
</example>


## MCP servers

Add to `~/.codex/config.toml`:

```toml
[mcp_servers.trigger-dev]
command = "npx"
args = ["trigger.dev@latest","mcp"]

[mcp_servers.trigger-dev.env]
"TRIGGER_ACCESS_TOKEN" = "${TRIGGER_ACCESS_TOKEN}"
"TRIGGER_API_URL" = "${TRIGGER_API_URL}"

[mcp_servers.n8n-mcp-external]
command = "npx"
args = ["n8n-mcp"]

[mcp_servers.n8n-mcp-external.env]
"MCP_MODE" = "stdio"
"LOG_LEVEL" = "error"
"DISABLE_CONSOLE_OUTPUT" = "true"
"N8N_API_URL" = "${N8N_API_URL}"
"N8N_API_KEY" = "${N8N_API_KEY}"

[mcp_servers.n8n-native-mcp]
url = "${N8N_NATIVE_MCP_URL}"
type = "http"

[mcp_servers.n8n-native-mcp.headers]
"Authorization" = "Bearer ${N8N_MCP_TOKEN}"

[mcp_servers.nocodb]
url = "${NOCODB_MCP_URL}"
type = "http"

[mcp_servers.nocodb.headers]
"xc-mcp-token" = "${NOCODB_MCP_TOKEN}"

[mcp_servers.postgresql-mcp]
url = "${POSTGRESQL_MCP_URL}"
type = "http"

[mcp_servers.postgresql-mcp.headers]
"Authorization" = "Bearer ${POSTGRESQL_MCP_TOKEN}"

[mcp_servers.nocobase-dev]
url = "${NOCOBASE_DEV_URL}/api/mcp"
type = "http"

[mcp_servers.nocobase-dev.headers]
"Authorization" = "Bearer ${NOCOBASE_DEV_API_KEY}"
"x-mcp-packages" = "plugin-data-source-main,plugin-data-source-manager,plugin-workflow*,plugin-acl,plugin-users,plugin-auth,plugin-client,plugin-flow-engine,plugin-ai"

```
