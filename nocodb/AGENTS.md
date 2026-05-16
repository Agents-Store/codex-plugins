# nocodb

> NocoDB database development plugin. Manage tables, records, columns, views, relations, formulas, rollups, lookups, filtering, sorting, search, aggregation, webhooks, and filter/sort management via MCP tools.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/nocodb

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

- **advanced-queries** — Advanced data queries — search, aggregate, group by, complex filters. This skill should be used when the user asks to perform complex queries, aggregate data, group records, or search with advanced filter logic.
- **column-field-management** — Column/field types, relations, lookups, rollups, formulas. This skill should be used when the user asks to add columns, configure field types, set up relations, lookups, rollups, or formulas.
- **examples** — Tool call patterns, end-to-end workflow examples, and scenario references. This skill should be used when the user needs reference implementations, complete examples, or tool call patterns.
- **record-operations** — Record CRUD, filtering, sorting, bulk operations. This skill should be used when the user asks to create, read, update, or delete records, filter or search data, bulk import, or aggregate values.
- **schema-design** — Schema design best practices — entity modeling, relation patterns, creation order, formulas, views. This skill should be used when the user asks to design a database schema, plan tables and relationships, or build CRM/ERP/project databases.
- **table-management** — Table CRUD operations — create, list, get, delete tables. This skill should be used when the user asks to create a table, list existing tables, or manage table structure.
- **view-management** — View types — Grid, Kanban, Gallery, Form, Calendar. This skill should be used when the user asks to create or configure views, set up a kanban board, build a form, or manage view filters and sorts.
- **webhook-management** — Webhook management — create, list, delete, and test webhooks for table events. This skill should be used when the user asks to set up webhooks, configure event notifications, or integrate with external systems.

## Subagents

Defined under `.codex/agents/` as TOML files:

- **nocodb-assistant** — Interactive NocoDB database assistant. Helps with table management, record operations, column configuration, view setup, relations, formulas, and schema design.

<example>
user: "Create a contacts table in NocoDB with name, email, and phone fields"
</example>
<example>
user: "Show me all records in the Orders table"
</example>
<example>
user: "Help me set up relations between Contacts and Deals tables"
</example>

- **nocodb-schema-designer** — Specialized NocoDB schema design agent. Designs database schemas with proper table structure, relations, lookups, rollups, formulas, and views. Use when planning multi-table NocoDB structures.

<example>
user: "Design a CRM database schema with contacts, companies, and deals"
</example>
<example>
user: "Plan a project management database with tasks, teams, and milestones"
</example>


## Workflows (canonical slash commands)

Codex CLI doesn't support custom slash commands — invoke these workflows via natural language. Each entry below is a prompt template you can adapt:

### `bulk-update`

Bulk update records in a NocoDB table

Arguments: `<table-name> --where <filter> --set <field=value>`

<details><summary>Prompt template</summary>

# Bulk Update

Update multiple records in a NocoDB table that match a filter.

## Arguments
Format: `<table-name> --where <filter> --set <field=value>`
- table-name: Name or ID of the table (required)
- --where: Filter to find records (required)
- --set: Field and value to update (required)

Parse from "$ARGUMENTS".

## Process

1. **Find table:**
   Use `list_tables` to resolve table name to ID.

2. **Find matching records:**
   Use `list_records` with the filter to preview what will be updated.

3. **Confirm and update:**
   Use `bulk_update_records` to update all matching records.

4. **Verify:**
   Use `list_records` to confirm updates.

## Example Usage
```
/bulk-update contacts --where "(Status,eq,Lead)" --set Status=Active
/bulk-update orders --where "(DueDate,lt,2024-01-01)~and(Status,eq,Pending)" --set Status=Overdue
```

</details>

### `create-column`

Add a column to a NocoDB table

Arguments: `<table> <column-name> <type> [--options <opt1,opt2>]`

<details><summary>Prompt template</summary>

# Create Column

Add a new column (field) to a NocoDB table.

## Arguments
Format: `<table> <column-name> <type> [--options <opt1,opt2>]`
- table: Table name or ID (required)
- column-name: Name for the new column (required)
- type: Column type — SingleLineText, Number, SingleSelect, Email, URL, Date, Checkbox, Currency, LinkToAnotherRecord, Lookup, Rollup, Formula (required)
- --options: For SingleSelect/MultiSelect — comma-separated options (optional)

Parse from "$ARGUMENTS".

## Process

1. **Resolve table:**
   ```
   list_tables()
   ```

2. **Check existing columns:**
   ```
   list_columns(table_id)
   ```
   Warn if column with same name exists.

3. **Create column:**
   ```
   create_column({
     table_id: <resolved_table_id>,
     name: <column-name>,
     type: <type>,
     options: <options if SingleSelect/MultiSelect>
   })
   ```

4. **Display result:**
   Show column ID, name, and type.

## Example Usage
```
/create-column "Contacts" "Phone" PhoneNumber
/create-column "Orders" "Priority" SingleSelect --options "Low,Medium,High,Critical"
/create-column "Products" "Price" Currency
```

</details>

### `create-record`

Create a new record in a NocoDB table

Arguments: `<table> [--data <field1=value1,field2=value2>]`

<details><summary>Prompt template</summary>

# Create Record

Create a new record in a NocoDB table.

## Arguments
Format: `<table> [--data <field1=value1,field2=value2>]`
- table: Table name or ID (required)
- --data: Comma-separated field=value pairs (optional, will prompt if missing)

Parse from "$ARGUMENTS".

## Process

1. **Resolve table:**
   ```
   list_tables()
   ```
   Find table by name or ID. If not found, show available tables.

2. **Get columns if no data provided:**
   ```
   list_columns(table_id)
   ```
   Show column names and types to help user provide data.

3. **Create record:**
   ```
   create_record({
     table_id: <resolved_table_id>,
     data: { <field>: <value>, ... }
   })
   ```

4. **Display created record:**
   Show record ID and field values.

## Example Usage
```
/create-record "Contacts" --data "Name=John Doe,Email=john@example.com,Status=Active"
/create-record "Orders" --data "Title=Order 001,Amount=1500,Status=New"
```

</details>

### `create-table`

Create a new table in NocoDB

Arguments: `<table-name> [--columns <col1:type,col2:type>]`

<details><summary>Prompt template</summary>

# Create Table

Create a new table in NocoDB.

## Arguments
Format: `<table-name> [--columns <col1:type,col2:type>]`
- table-name: Name of the table to create (required)
- --columns: Comma-separated column definitions as name:type pairs (optional)

Parse from "$ARGUMENTS".

## Process

1. **Check existing tables:**
   ```
   list_tables()
   ```
   If table with same name exists, warn user and stop.

2. **Create the table:**
   ```
   create_table({
     name: <table-name>,
     columns: <parsed columns or default [Title:SingleLineText]>
   })
   ```

3. **Display created table details:**
   Show table ID, name, and columns.

## Example Usage
```
/create-table "Contacts"
/create-table "Orders" --columns "Title:SingleLineText,Amount:Number,Status:SingleSelect,Email:Email"
/create-table "Products" --columns "Name:SingleLineText,Price:Currency,InStock:Checkbox"
```

</details>

### `create-view`

Create a new view for a NocoDB table

Arguments: `<table> <view-name> <type> [--group-by <field>]`

<details><summary>Prompt template</summary>

# Create View

Create a new view (Grid, Kanban, Gallery, Form, Calendar) for a NocoDB table.

## Arguments
Format: `<table> <view-name> <type> [--group-by <field>]`
- table: Table name or ID (required)
- view-name: Name for the new view (required)
- type: View type — Grid, Kanban, Gallery, Form, Calendar (required)
- --group-by: For Kanban — the SingleSelect field to group by (optional)

Parse from "$ARGUMENTS".

## Process

1. **Resolve table:**
   ```
   list_tables()
   ```

2. **Get columns (for Kanban/Calendar config):**
   ```
   list_columns(table_id)
   ```
   For Kanban, find the SingleSelect field. For Calendar, find the Date field.

3. **Create view:**
   ```
   create_view({
     table_id: <resolved_table_id>,
     title: <view-name>,
     type: <type>
   })
   ```

4. **Display result:**
   Show view ID, name, and type.

## Example Usage
```
/create-view "Contacts" "All Contacts" Grid
/create-view "Deals" "Pipeline" Kanban --group-by "Stage"
/create-view "Products" "Catalog" Gallery
/create-view "Leads" "Lead Form" Form
```

</details>

### `list-columns`

List columns in a NocoDB table

Arguments: `<table>`

<details><summary>Prompt template</summary>

# List Columns

List all columns (fields) in a NocoDB table.

## Arguments
Format: `<table>`
- table: Table name or ID (required)

Parse from "$ARGUMENTS".

## Process

1. **Resolve table:**
   ```
   list_tables()
   ```

2. **List columns:**
   ```
   list_columns(table_id)
   ```

3. **Display results:**
   Show column ID, name, type, and key config (options for select, formula for formula fields, etc.).

## Example Usage
```
/list-columns "Contacts"
/list-columns "Orders"
```

</details>

### `list-records`

List records from a NocoDB table with optional filters

Arguments: `<table> [--where <filter>] [--sort <field>] [--limit <n>]`

<details><summary>Prompt template</summary>

# List Records

List records from a NocoDB table with optional filtering, sorting, and pagination.

## Arguments
Format: `<table> [--where <filter>] [--sort <field>] [--limit <n>]`
- table: Table name or ID (required)
- --where: Filter in NocoDB format `(field,op,value)` (optional)
- --sort: Sort field, prefix with `-` for DESC (optional)
- --limit: Max records to return, default 25 (optional)

Parse from "$ARGUMENTS".

## Process

1. **Resolve table:**
   ```
   list_tables()
   ```
   Find table by name or ID.

2. **List records:**
   ```
   list_records({
     table_id: <resolved_table_id>,
     where: <filter or empty>,
     sort: <sort or empty>,
     limit: <limit or 25>,
     offset: 0
   })
   ```

3. **Display results:**
   Show records in a table format with key fields. Include total count and pagination info.

## Filter Format
- Single: `(Status,eq,Active)`
- Combined: `(Status,eq,Active)~and(Amount,gt,1000)`
- Operators: eq, neq, gt, lt, gte, lte, like, is, isnot

## Example Usage
```
/list-records "Contacts"
/list-records "Orders" --where "(Status,eq,New)" --sort "-CreatedAt" --limit 10
/list-records "Products" --where "(Price,gt,100)~and(InStock,eq,true)"
```

</details>

### `list-tables`

List all tables in the NocoDB base

<details><summary>Prompt template</summary>

# List Tables

List all tables in the current NocoDB base.

## Process

1. **Get all tables:**
   ```
   list_tables()
   ```

2. **Display results in a table:**
   Show ID, name, and column count for each table.

## Example Usage
```
/list-tables
```

</details>

### `list-views`

List views for a NocoDB table

Arguments: `<table>`

<details><summary>Prompt template</summary>

# List Views

List all views for a NocoDB table.

## Arguments
Format: `<table>`
- table: Table name or ID (required)

Parse from "$ARGUMENTS".

## Process

1. **Resolve table:**
   ```
   list_tables()
   ```

2. **List views:**
   ```
   list_views(table_id)
   ```

3. **Display results:**
   Show view ID, name, and type for each view.

## Example Usage
```
/list-views "Contacts"
/list-views "Orders"
```

</details>

### `search-records`

Search records in a NocoDB table

Arguments: `<table-name> <query>`

<details><summary>Prompt template</summary>

# Search Records

Search for records across all fields in a NocoDB table.

## Arguments
Format: `<table-name> <query>`
- table-name: Name or ID of the table (required)
- query: Search query text (required)

Parse from "$ARGUMENTS".

## Process

1. **Find table:**
   Use `list_tables` to resolve table name to ID.

2. **Search:**
   Use `search_records` with the query.

3. **Present results:**
   Display matching records in a formatted table.

## Example Usage
```
/search-records contacts "john doe"
/search-records orders "pending"
```

</details>
