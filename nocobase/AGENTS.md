# nocobase

> NocoBase platform development plugin. Expert guidance on collections, fields, relations, workflows, UI blocks, plugin development, MCP-powered page management, data operations, and collection inspection for NocoBase applications.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/nocobase

## MCP servers

Add to `~/.codex/config.toml`:

```toml
[mcp_servers.nocobase]
command = "npx"
args = ["-y","@nocobase/mcp-server"]

[mcp_servers.nocobase.env]
"NOCOBASE_URL" = "${NOCOBASE_URL}"
"NOCOBASE_EMAIL" = "${NOCOBASE_EMAIL}"
"NOCOBASE_PASSWORD" = "${NOCOBASE_PASSWORD}"

```

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **collections-design** — Collection design patterns — collection types, naming conventions, system fields, inheritance, tree structures. This skill should be used when the user asks to design a data model, create collections, plan entity relationships, or choose collection types.
- **data-operations** — Data CRUD operations — list, create, update, delete records. This skill should be used when the user asks to add records, query or filter data, update fields, delete entries, or bulk create records.
- **examples** — Application design scenarios and reference implementations. This skill should be used when the user needs complete application examples, tool call patterns, or end-to-end workflow references.
- **fields-relations** — Field types and relation configuration — basic, advanced, relation fields, foreign keys, through tables. This skill should be used when the user asks to configure fields, set up relations between collections, or manage foreign keys and through tables.
- **plugin-development** — Plugin scaffolding and development — lifecycle, server-side, client-side, migrations, testing. This skill should be used when the user asks to develop custom plugins, extend platform functionality, or scaffold plugin structure.
- **ui-configuration** — UI blocks, pages, actions, and field components. This skill should be used when the user asks to create a page, add a table or form block, build a dashboard, set up a kanban board, or configure menu structure.
- **workflows-automations** — Workflow engine — triggers, node types, conditions, variables, approval flows. This skill should be used when the user asks to create workflows, set up triggers, build approval flows, or automate business processes.

## Subagents

Defined under `.codex/agents/` as TOML files:

- **nocobase-assistant** — Interactive NocoBase platform expert. Manages collections, pages, UI blocks, data operations, and workflows for NocoBase applications.

<example>
user: "Help me design a CRM data model in NocoBase"
</example>
<example>
user: "Create a contacts page with a table block and filters"
</example>
<example>
user: "Set up a workflow that sends notifications on new orders"
</example>

- **nocobase-builder** — NocoBase application builder. Creates pages, configures UI blocks, manages data, and inspects collections.

<example>
user: "Build a page for the orders collection with a table and form"
</example>
<example>
user: "Import 50 sample contacts into NocoBase"
</example>


## Workflows (canonical slash commands)

Codex CLI doesn't support custom slash commands — invoke these workflows via natural language. Each entry below is a prompt template you can adapt:

### `create-page`

Create a NocoBase page with blocks

Arguments: `<page-title> [--collection <name>] [--block <table|form|details|kanban>]`

<details><summary>Prompt template</summary>

# Create Page

Create a NocoBase page with configured blocks, columns, and actions.

## Arguments
Format: `<page-title> [--collection <name>] [--block <table|form|details|kanban>]`
- page-title: Title of the page (required)
- --collection: Collection to bind to the block (optional)
- --block: Block type to add — table, form, details, kanban, calendar, gantt, chart (optional)

Parse from "$ARGUMENTS".

## Process

1. **Create the page:**
   Use `page_create` to create a new page with the given title.

2. **Add blocks (if collection specified):**
   Use `table_add` to add the requested block type bound to the collection.

3. **Configure columns:**
   Use `column_add` to add visible fields to the block.

4. **Add actions:**
   Use `action_add` to add standard actions (create, edit, delete, filter).

## Example Usage
```
/create-page "Contacts" --collection contacts --block table
/create-page "Dashboard"
/create-page "New Order" --collection orders --block form
```

</details>

### `create-plugin-scaffold`

Generate NocoBase plugin scaffold structure

Arguments: `<plugin-name> [--type <server|client|full>]`

<details><summary>Prompt template</summary>

# Create Plugin Scaffold

Generate the directory structure and boilerplate for a NocoBase plugin.

## Arguments
Format: `<plugin-name> [--type <server|client|full>]`
- plugin-name: Name of the plugin (required)
- --type: Plugin type — server (backend only), client (frontend only), full (both, default)

Parse from "$ARGUMENTS".

## Process

1. **Generate directory structure:**
   ```
   packages/plugins/@my-project/<plugin-name>/
   ├── package.json
   ├── src/
   │   ├── server/
   │   │   ├── index.ts
   │   │   ├── plugin.ts
   │   │   ├── collections/
   │   │   └── migrations/
   │   └── client/
   │       ├── index.ts
   │       ├── plugin.ts
   │       └── components/
   └── README.md
   ```

2. **Generate boilerplate files:**
   - package.json with NocoBase dependencies
   - Server plugin class with lifecycle hooks
   - Client plugin class with component registration

3. **Provide setup instructions.**

## Example Usage
```
/create-plugin-scaffold "custom-fields"
/create-plugin-scaffold "approval-workflow" --type server
/create-plugin-scaffold "dashboard-widgets" --type client
```

</details>

### `create-record`

Create a record in a NocoBase collection

Arguments: `<collection> <field1=value1> [field2=value2] ...`

<details><summary>Prompt template</summary>

# Create Record

Create a new record in a NocoBase collection.

## Arguments
Format: `<collection> <field1=value1> [field2=value2] ...`
- collection: Target collection name (required)
- field=value: Field name and value pairs (at least one required)

Parse from "$ARGUMENTS".

## Process

1. **Verify collection:**
   Use `collection_get` to check the collection exists and get available fields.

2. **Create record:**
   Use `data_create` with the collection name and field values.

3. **Confirm result:**
   Show the created record with its ID.

## Example Usage
```
/create-record contacts name="John Doe" email="john@example.com" status="Active"
/create-record orders total=1500 status="Draft"
```

</details>

### `design-collection`

Design a NocoBase collection schema

Arguments: `<collection-name> [--fields <field1:type,field2:type>]`

<details><summary>Prompt template</summary>

# Design Collection

Design a NocoBase collection (data model) schema with fields and relations.

## Arguments
Format: `<collection-name> [--fields <field1:type,field2:type>]`
- collection-name: Name of the collection (required)
- --fields: Comma-separated field definitions as name:type pairs (optional)

Parse from "$ARGUMENTS".

## Process

1. **Analyze requirements:**
   - What entity does this collection represent?
   - What fields are needed?
   - What relations to other collections?

2. **Generate schema:**
   Define collection with all fields, types, validations, and relation configuration.

3. **Provide implementation guidance:**
   Show the JSON configuration or UI steps to create the collection in NocoBase.

## Example Usage
```
/design-collection "contacts" --fields "name:string,email:email,phone:phone,status:select"
/design-collection "orders"
/design-collection "products" --fields "title:string,price:decimal,category:belongsTo"
```

## Notes
This is a knowledge-only command. If NocoBase MCP tools are available in your session, they will be used to create the collection directly.

</details>

### `design-ui-block`

Design a NocoBase UI block layout

Arguments: `<collection> <block-type>`

<details><summary>Prompt template</summary>

# Design UI Block

Design a NocoBase UI block for a collection — table, form, details, kanban, calendar, or chart.

## Arguments
Format: `<collection> <block-type>`
- collection: Target collection name (required)
- block-type: Block type — table, form, details, kanban, calendar, gantt, chart (required)

Parse from "$ARGUMENTS".

## Process

1. **Analyze collection:**
   Review collection fields and relations to determine best configuration.

2. **Design block:**
   - Select visible fields and their order
   - Configure filters and default sort
   - Set up actions (create, edit, delete, export)
   - Configure field components

3. **Provide implementation guidance.**

## Example Usage
```
/design-ui-block "contacts" table
/design-ui-block "deals" kanban
/design-ui-block "events" calendar
/design-ui-block "orders" form
```

</details>

### `list-collections`

List all NocoBase collections

Arguments: `--details`

<details><summary>Prompt template</summary>

# List Collections

List all collections (data models) in the NocoBase instance.

## Arguments
Format: `[--details]`
- --details: Show field details for each collection (optional)

Parse from "$ARGUMENTS".

## Process

1. **List collections:**
   Use `collection_list` to get all collections.

2. **Show details (if requested):**
   For each collection of interest, use `collection_get` to show fields, relations, and configuration.

3. **Present results:**
   Display collections in a structured table with name, title, and field count.

## Example Usage
```
/list-collections
/list-collections --details
```

</details>

### `list-records`

List records from a NocoBase collection

Arguments: `<collection> [--limit <n>] [--filter <field=value>]`

<details><summary>Prompt template</summary>

# List Records

List records from a NocoBase collection with optional filtering.

## Arguments
Format: `<collection> [--limit <n>] [--filter <field=value>]`
- collection: Target collection name (required)
- --limit: Maximum records to return (optional, default: 20)
- --filter: Filter by field value (optional)

Parse from "$ARGUMENTS".

## Process

1. **Get collection info:**
   Use `collection_get` to understand available fields.

2. **List records:**
   Use `data_list` with the collection name, limit, and filter parameters.

3. **Present results:**
   Display records in a formatted table with key fields.

## Example Usage
```
/list-records contacts
/list-records orders --limit 50 --filter status=Draft
/list-records products --filter category=Electronics
```

</details>

### `plan-workflow`

Plan a NocoBase workflow automation

Arguments: `<description>`

<details><summary>Prompt template</summary>

# Plan Workflow

Design a NocoBase workflow automation with trigger, nodes, and conditions.

## Arguments
Format: `<description>`
- description: What the workflow should do (required)

Parse from "$ARGUMENTS".

## Process

1. **Analyze requirements:**
   - What triggers the workflow? (collection event, schedule, manual)
   - What actions should occur?
   - What conditions or branches are needed?

2. **Design workflow:**
   - Define trigger type and configuration
   - Plan node chain (condition, query, create, update, request, etc.)
   - Define variable mappings between nodes

3. **Provide implementation guidance:**
   Show workflow configuration for NocoBase.

## Example Usage
```
/plan-workflow "Send email notification when a new order is created"
/plan-workflow "Auto-assign tasks to team members based on priority"
/plan-workflow "Generate daily sales report and send to Slack"
```

</details>
