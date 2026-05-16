# n8n

> n8n workflow automation plugin. Manage workflows, execute automations, configure nodes, handle credentials, monitor executions, expression syntax, node configuration patterns, and code node best practices via MCP tools.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/n8n

## MCP servers

Add to `~/.codex/config.toml`:

```toml
[mcp_servers.n8n]
url = "${N8N_MCP_URL}"
type = "http"

[mcp_servers.n8n.headers]
"Authorization" = "Bearer ${N8N_MCP_TOKEN}"

```

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **code-patterns** — Code node patterns — JavaScript and Python data transformation, API processing, date handling, binary data. This skill should be used when the user asks to write code in Code nodes, transform data with JavaScript or Python, or process binary data.
- **credential-tag-management** — Credentials and tags management. This skill should be used when the user asks to create or manage credentials, list service connections, organize workflows with tags, or configure authentication.
- **examples** — Tool call patterns, end-to-end workflow examples, and scenario references. This skill should be used when the user needs reference implementations, complete examples, or tool call patterns.
- **execution-monitoring** — Workflow execution, monitoring, and debugging. This skill should be used when the user asks to run a workflow, check execution status, view execution history, or debug workflow errors.
- **expression-syntax** — Expression syntax — variables, methods, JMESPath, data references. This skill should be used when the user asks to write expressions, reference data between nodes, use JMESPath, or debug expression errors.
- **node-configuration** — Node configuration — HTTP Request, Code, IF, Switch, Merge, Split In Batches, error handling. This skill should be used when the user asks to configure a specific node type, set up authentication, write conditions, or add error handling.
- **workflow-creation** — Workflow creation — node definitions, connections, triggers, workflow structure. This skill should be used when the user asks to create a new workflow, build an automation, set up triggers, or scaffold a workflow structure.
- **workflow-editing** — Workflow editing — adding/removing nodes, updating connections, modifying node parameters. This skill should be used when the user asks to edit, modify, or update an existing workflow, add or remove nodes, or change node settings.

## Subagents

Defined under `.codex/agents/` as TOML files:

- **n8n-assistant** — Interactive n8n workflow automation assistant. Helps with creating, editing, executing, and monitoring workflows, managing credentials and tags.

<example>
user: "List all my n8n workflows and show which ones are active"
</example>
<example>
user: "Help me debug why my workflow execution failed"
</example>
<example>
user: "Set up API credentials for my Slack integration"
</example>

- **n8n-workflow-builder** — Specialized n8n workflow builder. Designs and builds workflow automations with proper node configuration and connections. Use when creating or editing complex workflows.

<example>
user: "Build a webhook workflow that processes incoming orders"
</example>
<example>
user: "Create a scheduled workflow that syncs data from an API every hour"
</example>


## Workflows (canonical slash commands)

Codex CLI doesn't support custom slash commands — invoke these workflows via natural language. Each entry below is a prompt template you can adapt:

### `activate-workflow`

Activate or deactivate an n8n workflow

Arguments: `<workflow> [--off]`

<details><summary>Prompt template</summary>

# Activate Workflow

Activate a workflow (or deactivate with --off flag).

## Arguments
Format: `<workflow> [--off]`
- workflow: Workflow name or ID (required)
- --off: Deactivate instead of activate (optional)

Parse from "$ARGUMENTS".

## Process

1. **Resolve workflow:**
   ```
   list_workflows()
   ```

2. **Toggle activation:**
   ```
   activate_workflow(workflow_id)   // or deactivate_workflow if --off
   ```

3. **Confirm status change.**

## Example Usage
```
/activate-workflow "Data Sync"
/activate-workflow "API Handler" --off
```

</details>

### `create-workflow`

Create a new n8n workflow

Arguments: `<name> [--trigger <webhook|schedule|manual>]`

<details><summary>Prompt template</summary>

# Create Workflow

Create a new workflow in n8n.

## Arguments
Format: `<name> [--trigger <webhook|schedule|manual>]`
- name: Workflow name (required)
- --trigger: Trigger type — webhook, schedule, or manual (default: manual)

Parse from "$ARGUMENTS".

## Process

1. **Create workflow with trigger:**
   ```
   create_workflow({
     name: <name>,
     nodes: [<trigger_node based on type>],
     connections: {}
   })
   ```

2. **Display result:**
   Show workflow ID, name, and trigger configuration.

## Example Usage
```
/create-workflow "Data Sync"
/create-workflow "API Handler" --trigger webhook
/create-workflow "Daily Report" --trigger schedule
```

</details>

### `debug-workflow`

Debug a failed n8n workflow execution

Arguments: `<workflow-id> [--execution <execution-id>]`

<details><summary>Prompt template</summary>

# Debug Workflow

Debug a failed n8n workflow execution — find the failing node, error message, and suggest fixes.

## Arguments
Format: `<workflow-id> [--execution <execution-id>]`
- workflow-id: ID of the workflow to debug (required)
- --execution: Specific execution ID to debug (optional, defaults to last failed)

Parse from "$ARGUMENTS".

## Process

1. **Find failed executions:**
   Use `list_executions` with status="error" to find recent failures.

2. **Get execution details:**
   Use `get_execution` to see node-by-node data and identify the failing node.

3. **Get workflow structure:**
   Use `get_workflow` to understand the workflow configuration.

4. **Diagnose:**
   - Identify which node failed
   - What error message was returned
   - What input data the failing node received
   - Suggest fixes

## Example Usage
```
/debug-workflow wf_abc123
/debug-workflow wf_abc123 --execution exec_xyz789
```

</details>

### `execute-workflow`

Execute an n8n workflow

Arguments: `<workflow> [--data <json>]`

<details><summary>Prompt template</summary>

# Execute Workflow

Execute a workflow manually with optional input data.

## Arguments
Format: `<workflow> [--data <json>]`
- workflow: Workflow name or ID (required)
- --data: JSON input data for the trigger node (optional)

Parse from "$ARGUMENTS".

## Process

1. **Resolve workflow:**
   ```
   list_workflows()
   ```
   Find workflow by name or ID.

2. **Execute:**
   ```
   execute_workflow({
     workflow_id: <resolved_id>,
     data: <parsed JSON or empty>
   })
   ```

3. **Get execution result:**
   ```
   get_execution(execution_id)
   ```
   Show execution status, duration, and node outputs.

## Example Usage
```
/execute-workflow "Data Sync"
/execute-workflow "API Handler" --data '{"name": "test", "value": 42}'
```

</details>

### `get-workflow`

Get n8n workflow details

Arguments: `<workflow-id>`

<details><summary>Prompt template</summary>

# Get Workflow

Get detailed information about an n8n workflow — nodes, connections, settings, and active status.

## Arguments
Format: `<workflow-id>`
- workflow-id: ID of the workflow (required)

Parse from "$ARGUMENTS".

## Process

1. **Get workflow:**
   Use `get_workflow` to fetch full workflow data.

2. **Present details:**
   - Workflow name and active status
   - List of nodes with types
   - Connection map
   - Trigger type
   - Tags and settings

## Example Usage
```
/get-workflow wf_abc123
```

</details>

### `list-credentials`

List available n8n credentials

<details><summary>Prompt template</summary>

# List Credentials

List all available credentials in the n8n instance.

## Process

1. **List credentials:**
   ```
   list_credentials()
   ```

2. **Display results:**
   Show credential ID, name, and type for each.

## Example Usage
```
/list-credentials
```

</details>

### `list-executions`

List recent workflow executions

Arguments: `[--workflow <name>] [--status <success|error|waiting>] [--limit <n>]`

<details><summary>Prompt template</summary>

# List Executions

List recent workflow executions with optional filters.

## Arguments
Format: `[--workflow <name>] [--status <success|error|waiting>] [--limit <n>]`
- --workflow: Filter by workflow name or ID (optional)
- --status: Filter by status — success, error, waiting (optional)
- --limit: Max results, default 20 (optional)

Parse from "$ARGUMENTS".

## Process

1. **Resolve workflow if specified:**
   ```
   list_workflows()
   ```

2. **List executions:**
   ```
   list_executions({
     workflow_id: <id or empty>,
     status: <status or empty>,
     limit: <limit or 20>
   })
   ```

3. **Display results:**
   Show execution ID, workflow name, status, start time, and duration.

## Example Usage
```
/list-executions
/list-executions --workflow "Data Sync" --status error
/list-executions --limit 50
```

</details>

### `list-tags`

List workflow tags

<details><summary>Prompt template</summary>

# List Tags

List all workflow tags in the n8n instance.

## Process

1. **List tags:**
   ```
   list_tags()
   ```

2. **Display results:**
   Show tag ID, name, and workflow count.

## Example Usage
```
/list-tags
```

</details>

### `list-workflows`

List all n8n workflows

Arguments: `[--active] [--tag <tag>]`

<details><summary>Prompt template</summary>

# List Workflows

List all workflows in the n8n instance.

## Arguments
Format: `[--active] [--tag <tag>]`
- --active: Show only active workflows (optional)
- --tag: Filter by tag name (optional)

Parse from "$ARGUMENTS".

## Process

1. **List workflows:**
   ```
   list_workflows()
   ```

2. **Display results:**
   Show ID, name, active status, and tags for each workflow. Filter by arguments if provided.

## Example Usage
```
/list-workflows
/list-workflows --active
/list-workflows --tag "production"
```

</details>
