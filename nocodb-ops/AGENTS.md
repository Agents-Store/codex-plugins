# nocodb-ops

> NocoDB ops plugin for Agents Store. Record management, views, reports, filtering, search, and data import/export for business users via MCP tools and CLI.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/nocodb-ops

## MCP servers

Add to `~/.codex/config.toml`:

```toml
[mcp_servers.nocodb]
url = "${NOCODB_MCP_URL}"
type = "http"

[mcp_servers.nocodb.headers]
"xc-mcp-token" = "${NOCODB_MCP_TOKEN}"

```

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **cli-reference** — NocoDB CLI commands and nc command reference from the official NocoDB agent-skills package. Use when:
- "NocoDB CLI commands"
- "nc command reference"
- "NocoDB agent-skills"
- "what CLI commands are available"
- "how to use nc command"

- **examples** — NocoDB workflow examples, scenario walkthroughs, and practical patterns. Use when:
- "show me a NocoDB example"
- "workflow examples"
- "scenario walkthroughs"
- "how do I use NocoDB for..."
- "NocoDB use case"

- **import-export** — Import data into NocoDB tables and export records out. Use when:
- "import CSV into NocoDB"
- "load data into this table"
- "bulk create records"
- "export records to CSV"
- "migrate data between tables"
- "import JSON data"
- "extract all records"
- "download table data"

- **mcp-patterns** — NocoDB MCP tools reference -- available tools, parameters, and usage patterns. Use when:
- "what NocoDB tools are available?"
- "how do I query records?"
- "show me NocoDB MCP parameters"
- "which tool do I use for..."
- "NocoDB tool reference"

- **record-management** — Create, read, update, and delete NocoDB records. Use when:
- "add a new record"
- "create entries in NocoDB"
- "update a record"
- "delete records"
- "bulk import data"
- "search and edit records"
- "how many records match..."

- **search-filter** — NocoDB filter syntax reference for searching, filtering, and sorting records. Use when:
- "filter records"
- "search for records where"
- "NocoDB where clause"
- "how to filter by date"
- "sort results"
- "query syntax"
- "find records matching"
- "filter by status"
- "records created this week"
- "combine multiple filters"

- **setup** — Verify NocoDB connection and MCP setup. Use when:
- "check my NocoDB connection"
- "verify MCP is working"
- "test NocoDB setup"
- "is NocoDB connected?"
- "troubleshoot NocoDB access"

- **troubleshoot** — Diagnose and fix NocoDB errors, connection issues, and MCP problems. Use when:
- "NocoDB not working"
- "connection error"
- "getting 401 error"
- "MCP tool not responding"
- "debug NocoDB"
- "filter not working"
- "timeout error"

- **views-and-reports** — Build reports, summaries, and dashboards from NocoDB data. Use when:
- "create a report"
- "summarize this table"
- "show sales by region"
- "build a dashboard"
- "aggregate data"
- "what views exist on this table?"
- "kanban board"
- "monthly summary"
- "count by category"
- "average order value"


## Subagents

Defined under `.codex/agents/` as TOML files:

- **data-assistant** — Use this agent when the user needs help with NocoDB data operations — finding records, building reports, creating views, importing/exporting data, or performing day-to-day business tasks.

<example>
Context: User wants to find specific data
user: "Find all customers who signed up last month and show me their order totals"
assistant: "I'll use the data-assistant agent to search and aggregate the data."
<commentary>
Business user needs to find and summarize data — agent queries records and presents results.
</commentary>
</example>

<example>
Context: User wants to build a report
user: "Create a weekly sales summary grouped by region"
assistant: "I'll use the data-assistant agent to build the report."
<commentary>
Business user needs a report — agent uses aggregate and queryRecords to compile the summary.
</commentary>
</example>

<example>
Context: User needs to import data
user: "Import these 200 contacts from the spreadsheet into NocoDB"
assistant: "I'll use the data-assistant agent to handle the bulk import."
<commentary>
Business user needs data operations — agent validates and batch-creates records.
</commentary>
</example>


## Workflows (canonical slash commands)

Codex CLI doesn't support custom slash commands — invoke these workflows via natural language. Each entry below is a prompt template you can adapt:

### `build-report`

Build an aggregation report from NocoDB table data

Arguments: `<table-name> [aggregation-type] [field]`

<details><summary>Prompt template</summary>

# Build Report

Build an aggregation report from a NocoDB table.

## Arguments

Parse from "$ARGUMENTS":
- `table-name` (required): Name or ID of the table
- `aggregation-type` (optional): sum, count, avg, min, max, median (default: count)
- `field` (optional): Field to aggregate on

## Process

1. Run `getTablesList` to resolve the table name to an ID.
2. Run `getTableSchema` to discover numeric and countable fields.
3. Run `aggregate` with appropriate aggregation type and field.
4. If no field specified, run count aggregation on the whole table.
5. Present results in a clear summary format.
6. Suggest additional aggregations or filters for deeper analysis.

## Example Usage

```
/build-report Orders sum Amount
/build-report Contacts count
/build-report Products avg Price
/build-report Deals max Value
```

</details>

### `create-record`

Create a new record in a NocoDB table

Arguments: `<table-name> <field=value pairs>`

<details><summary>Prompt template</summary>

# Create Record

Create a new record in a NocoDB table.

## Arguments

Parse from "$ARGUMENTS":
- `table-name` (required): Name or ID of the table
- Field values: key=value pairs or JSON object

## Process

1. Run `getTablesList` to resolve the table name to an ID.
2. Run `getTableSchema` to discover required fields and types.
3. Map the provided field values to the table schema.
4. Run `createRecords` with the mapped data.
5. Confirm creation and display the new record.

## Example Usage

```
/create-record Contacts Name="Jane Smith" Email="jane@example.com" Status=Active
/create-record Orders Product="Widget A" Quantity=10 Status=Pending
```

</details>

### `create-view`

Create a new view for a NocoDB table

Arguments: `<table-name> <view-type> [title]`

<details><summary>Prompt template</summary>

# Create View

Describe how to create a new view for a NocoDB table. Since NocoDB MCP does not expose view management tools directly, guide the user through the NocoDB web interface or CLI.

## Arguments

Parse from "$ARGUMENTS":
- `table-name` (required): Name or ID of the table
- `view-type` (required): grid, kanban, gallery, form, or calendar
- `title` (optional): Name for the new view

## Process

1. Run `getTablesList` to resolve the table name.
2. Run `getTableSchema` to list existing views and columns.
3. Based on view type, recommend:
   - **Grid**: default working view, suggest useful filters and sorts
   - **Kanban**: identify SingleSelect columns suitable for grouping
   - **Gallery**: identify Attachment columns for cover images
   - **Form**: suggest which fields to include as required
   - **Calendar**: identify Date/DateTime columns
4. Provide step-by-step instructions for creating the view in the NocoDB UI.
5. If CLI is available, show the `nc view:create` command.

## Example Usage

```
/create-view Deals kanban "Deal Pipeline"
/create-view Contacts form "New Contact Form"
/create-view Events calendar "Event Schedule"
```

</details>

### `list-records`

List records from a NocoDB table with optional filtering

Arguments: `<table-name> [where-filter]`

<details><summary>Prompt template</summary>

# List Records

Query records from a NocoDB table with optional filtering and sorting.

## Arguments

Parse from "$ARGUMENTS":
- `table-name` (required): Name or ID of the table
- `where-filter` (optional): Filter in NocoDB syntax, e.g. `(Status,eq,Active)`

## Process

1. Run `getTablesList` to resolve the table name to an ID.
2. Run `getTableSchema` to discover field names and types.
3. Run `queryRecords` with the table ID and optional where filter.
4. Display results in a formatted table showing key fields.
5. Report total count and pagination info if more records exist.

## Example Usage

```
/list-records Contacts
/list-records Orders (Status,eq,Pending)
/list-records Deals (Amount,gt,5000)~and(Stage,eq,Negotiation)
```

</details>

### `list-tables`

List all tables in the NocoDB base

<details><summary>Prompt template</summary>

# List Tables

List all tables in the current NocoDB base.

## Process

1. Run `getTablesList` to fetch all accessible tables.
2. Display results in a table with ID, name, and description.
3. If no tables found, suggest checking the connection with the setup skill.

</details>

### `search-records`

Search records in a NocoDB table by keyword

Arguments: `<table-name> <query>`

<details><summary>Prompt template</summary>

# Search Records

Search for records containing a keyword across text fields.

## Arguments

Parse from "$ARGUMENTS":
- `table-name` (required): Name or ID of the table
- `query` (required): Search text to find

## Process

1. Run `getTablesList` to resolve the table name to an ID.
2. Run `getTableSchema` to identify text fields (SingleLineText, LongText, Email, URL, PhoneNumber).
3. Build a `where` filter using `like` operator across text fields combined with `~or`.
4. Run `queryRecords` with the constructed filter.
5. Display matching records in a formatted table.

## Example Usage

```
/search-records Contacts "john doe"
/search-records Orders "pending"
/search-records Products "widget"
```

</details>
