# directus-dev

> Directus development plugin. Knowledge base for working with Directus MCP tools (12 tools), REST API, and @directus/sdk. Covers collections, items, fields, relations, files, flows, operations, and schema design.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/directus-dev

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **api-reference** — Directus REST API endpoints, curl examples, authentication patterns, schema migration pipeline. This skill should be used when the user asks for "Directus API endpoints", "Directus REST API", "Directus curl examples", "Directus API documentation", "Directus HTTP requests", or needs specific endpoint details for scripting.
- **examples** — End-to-end workflow examples, tool call patterns, and scenario walkthroughs for Directus MCP. This skill should be used when the user needs complete working examples, reference implementations, step-by-step walkthroughs, or tool call patterns for Directus.
- **field-relations** — Field types, relationship configuration — M2O, O2M, M2M, M2A, translations, files. This skill should be used when the user asks to add fields, configure field types, set up relations, create relationships between Directus collections, or design M2O/O2M/M2M/M2A structures.
- **file-management** — File and asset management — upload via URL, organize folders, query file metadata, retrieve base64 assets. This skill should be used when the user asks to manage files, upload images, import files from URLs, organize folders, query file metadata, or retrieve assets in Directus.
- **flow-automation** — Flow automation — triggers, operations, data chains, event hooks, webhooks, schedules. This skill should be used when the user asks to create automation flows, set up triggers, configure operations, build event-driven workflows, schedule tasks, or trigger flows programmatically in Directus.
- **item-operations** — Item CRUD operations — create, read, update, delete items, filtering, sorting, deep queries, aggregation, batch operations. This skill should be used when the user asks to create, read, update, or delete items, filter or search data, query with relations, aggregate values, or perform batch operations in Directus.
- **mcp-tools** — All Directus MCP tools reference — action patterns, parameters, query system. This skill should be used when the user asks about "Directus MCP tools", "which Directus tools are available", "how to use Directus MCP", "Directus tool parameters", or needs to know which MCP operations are available for Directus and how to use them correctly.
- **schema-design** — Schema design best practices — data modeling, collection planning, system fields, display templates, singletons, folders, versioning. This skill should be used when the user asks to design a data model, plan collections, create a database schema, or build a CMS/e-commerce/project database structure in Directus.
- **sdk-patterns** — @directus/sdk patterns — composable client, TypeScript types, CRUD operations, authentication, real-time subscriptions. This skill should be used when the user asks about "Directus SDK", "@directus/sdk", "Directus client library", "Directus TypeScript", or needs code patterns for integrating Directus into a JavaScript/TypeScript project.
- **troubleshoot** — Directus troubleshooting — common errors, diagnostics, MCP connection issues, permission problems, schema conflicts. This skill should be used when the user encounters "Directus errors", "Directus not working", "Directus connection issues", "Directus 403/422/500 errors", or needs to diagnose and fix problems.

## Subagents

Defined under `.codex/agents/` as TOML files:

- **directus-assistant** — Interactive Directus assistant. Helps with collection management, item operations, field configuration, relation setup, file management, flow automation, and schema design via MCP tools.

<example>
Context: User wants to explore their Directus instance
user: "Show me all collections in my Directus instance"
assistant: "I'll use the directus-assistant agent to explore the schema."
<commentary>
User wants to discover what's in their Directus instance.
</commentary>
</example>

<example>
Context: User wants to create content
user: "Create 5 blog posts in the articles collection with different topics"
assistant: "I'll use the directus-assistant agent to create the items."
<commentary>
User needs to create items in a Directus collection.
</commentary>
</example>

<example>
Context: User wants to set up a relation
user: "Set up a many-to-many relation between products and categories"
assistant: "I'll use the directus-assistant agent to create the M2M relation."
<commentary>
User needs help with Directus relationship configuration.
</commentary>
</example>

<example>
Context: User wants to query data
user: "Show me all published posts with their authors and tags"
assistant: "I'll use the directus-assistant agent to query the data."
<commentary>
User needs to read items with relational data.
</commentary>
</example>

<example>
Context: User wants to set up automation
user: "Create a flow that sends a notification when a new order is placed"
assistant: "I'll use the directus-assistant agent to build the automation flow."
<commentary>
User needs to create a Directus flow with operations.
</commentary>
</example>

- **directus-schema-architect** — Specialized Directus schema design agent. Designs data models with collections, fields, relations (M2O, O2M, M2M, M2A), system fields, translations, and content versioning. Use when planning multi-collection Directus structures.

<example>
Context: User wants to design a complex data model
user: "Design a blog CMS schema with posts, categories, tags, and authors"
assistant: "I'll use the directus-schema-architect agent to design the data model."
<commentary>
User needs a multi-collection schema design with various relation types.
</commentary>
</example>

<example>
Context: User wants to plan an e-commerce backend
user: "Plan an e-commerce data model with products, variants, and orders"
assistant: "I'll use the directus-schema-architect agent to plan the schema."
<commentary>
Complex schema requiring hierarchical categories, M2M relations, and order management.
</commentary>
</example>

<example>
Context: User wants to restructure existing schema
user: "My Directus schema is messy, help me redesign it properly"
assistant: "I'll use the directus-schema-architect agent to analyze and redesign."
<commentary>
Schema refactoring requires understanding current state and planning improvements.
</commentary>
</example>


## Workflows (canonical slash commands)

Codex CLI doesn't support custom slash commands — invoke these workflows via natural language. Each entry below is a prompt template you can adapt:

### `create-collection`

Create a new Directus collection with optional fields

Arguments: `<name> [--fields <field1:type,field2:type>] [--icon <icon>]`

<details><summary>Prompt template</summary>

# Create Collection

## Arguments

- `name` (required) — Collection name (snake_case recommended)
- `--fields <field1:type,...>` — Comma-separated field definitions (e.g., `title:string,content:text,price:decimal`)
- `--icon <icon>` — Material icon name (default: `box`)

## Process

1. Parse arguments
2. Call `schema` tool (no params) to check collection doesn't already exist
3. Call `collections` tool to create the collection:
   ```json
   {
     "action": "create",
     "data": [{
       "collection": "<name>",
       "schema": {},
       "meta": {
         "icon": "<icon>",
         "display_template": "{{<first_string_field>}}"
       }
     }]
   }
   ```
4. If `--fields` provided, parse and create fields:
   ```json
   {
     "action": "create",
     "collection": "<name>",
     "data": [
       { "field": "<name>", "type": "<type>", "meta": { "interface": "<auto-mapped>" } }
     ]
   }
   ```
   Auto-map types to interfaces: string→input, text→input-rich-text-md, integer→input, decimal→input, boolean→boolean, datetime→datetime, json→input-code
5. Display the created collection and its fields
6. Suggest adding relations or more fields

## Example

Input: `products --fields name:string,description:text,price:decimal,active:boolean --icon inventory_2`

</details>

### `create-item`

Create a new item in a Directus collection

Arguments: `<collection> <field=value> [field=value...]`

<details><summary>Prompt template</summary>

# Create Item

## Arguments

- `collection` (required) — Collection name
- `field=value` pairs — Field values for the new item

## Process

1. Parse collection name and field=value pairs from input
2. Call `schema` tool with `keys: ["<collection>"]` to check required fields
3. Warn if any required fields are missing from the input
4. Build data object from field=value pairs
5. Call `items` tool with:
   ```json
   {
     "action": "create",
     "collection": "<collection>",
     "data": [{ "field1": "value1", "field2": "value2" }]
   }
   ```
   **Remember: data is ALWAYS an array**
6. Display the created item with its ID
7. Suggest viewing the item or creating more

## Example

Input: `posts title="My New Post" status=draft content="Post content here"`

```json
{
  "action": "create",
  "collection": "posts",
  "data": [{
    "title": "My New Post",
    "status": "draft",
    "content": "Post content here"
  }]
}
```

</details>

### `explore-schema`

Explore the Directus schema — list all collections or get detailed field info for a specific collection

Arguments: `collection-name`

<details><summary>Prompt template</summary>

# Explore Schema

Explore the Directus data model.

## Process

1. If this is the first call in the session, call `system-prompt` tool first (no params)

2. **No argument provided:**
   - Call `schema` tool with no parameters (discovery mode)
   - Display all collections as a table: name, icon, notes
   - Group by collection folders if present
   - Show total collection count

3. **Collection name provided:**
   - Call `schema` tool with `keys: ["<collection-name>"]`
   - Display fields as a table: name, type, interface, required, notes
   - List relations: field → related collection (type)
   - Show display template if set

## Output Format

**Discovery mode:**
```
| Collection | Icon | Notes |
|-----------|------|-------|
| posts     | article | Blog posts |
| authors   | person  | Content authors |
```

**Detailed mode:**
```
## posts

| Field | Type | Interface | Required |
|-------|------|-----------|----------|
| title | string | input | Yes |
| content | text | rich-text-md | No |

Relations:
- author → authors (M2O)
- tags ↔ tags (M2M via posts_tags)
```

</details>

### `list-collections`

List all collections in the Directus instance

<details><summary>Prompt template</summary>

# List Collections

## Process

1. Call `schema` tool with no parameters (discovery mode)
2. Display collections grouped by folders (if any)
3. Show collection name, icon, and notes for each
4. Show total count

## Output

Display as a clean table. Separate system collections (directus_*) from user collections if both are visible.

</details>

### `list-files`

List files in Directus with optional folder or type filter

Arguments: `[--folder <name>] [--type <mime-prefix>]`

<details><summary>Prompt template</summary>

# List Files

## Arguments

- `--folder <name>` — Filter by folder name
- `--type <mime-prefix>` — Filter by MIME type prefix (e.g., `image`, `application/pdf`)

## Process

1. If `--folder` provided:
   - Call `folders` tool with `action: "read"` to find folder UUID by name
   - Use folder UUID in file filter

2. Build file query:
   ```json
   {
     "action": "read",
     "query": {
       "fields": ["id", "title", "filename_download", "type", "filesize", "width", "height", "folder.name"],
       "filter": {},
       "sort": ["-uploaded_on"],
       "limit": 25
     }
   }
   ```
   - Add folder filter: `{ "folder": { "_eq": "<uuid>" } }`
   - Add type filter: `{ "type": { "_starts_with": "<prefix>/" } }`

3. Call `files` tool with the query
4. Display as table: title, filename, type, size, dimensions, folder
5. Format filesize as human-readable (KB, MB)

## Example

Input: `--type image --folder Products`

Lists all image files in the "Products" folder.

</details>

### `list-flows`

List all automation flows in Directus

<details><summary>Prompt template</summary>

# List Flows

## Process

1. Call `flows` tool with:
   ```json
   {
     "action": "read",
     "query": {
       "fields": ["id", "name", "status", "trigger", "description", "icon", "color"],
       "sort": ["name"]
     }
   }
   ```
2. Display flows as a table: name, status (active/inactive), trigger type, description
3. Show total count
4. Highlight active vs inactive flows

## Output

```
| Name | Status | Trigger | Description |
|------|--------|---------|-------------|
| Welcome Email | active | event | Send email on user create |
| Daily Report | active | schedule | Generate daily metrics |
| Cleanup | inactive | schedule | Remove old drafts |
```

</details>

### `list-items`

List items from a Directus collection with optional filters

Arguments: `<collection> [--filter <field=value>] [--sort <field>] [--limit <n>]`

<details><summary>Prompt template</summary>

# List Items

## Arguments

- `collection` (required) — Collection name
- `--filter <field=value>` — Filter by field value (e.g., `--filter status=published`)
- `--sort <field>` — Sort field (prefix `-` for descending, e.g., `--sort -date_created`)
- `--limit <n>` — Max results (default: 25)

## Process

1. Parse arguments from the input
2. Call `schema` tool with `keys: ["<collection>"]` to understand fields
3. Build query object:
   - Select the most useful fields based on schema (avoid fetching all fields on wide collections)
   - Apply filter if provided: `{ "<field>": { "_eq": "<value>" } }`
   - Apply sort if provided
   - Apply limit (default 25)
4. Call `items` tool with `action: "read"`, `collection`, and `query`
5. Display results as a formatted table
6. Show total count if available

## Example

Input: `posts --filter status=published --sort -date_created --limit 10`

```json
{
  "action": "read",
  "collection": "posts",
  "query": {
    "fields": ["id", "title", "status", "date_created"],
    "filter": { "status": { "_eq": "published" } },
    "sort": ["-date_created"],
    "limit": 10
  }
}
```

</details>

### `search-items`

Search items across a Directus collection using full-text search

Arguments: `<collection> <query>`

<details><summary>Prompt template</summary>

# Search Items

## Arguments

- `collection` (required) — Collection name
- `query` (required) — Search text

## Process

1. Parse collection name and search query
2. Call `schema` tool with `keys: ["<collection>"]` to determine best display fields
3. Call `items` tool with:
   ```json
   {
     "action": "read",
     "collection": "<collection>",
     "query": {
       "search": "<query>",
       "fields": ["id", "<main_field>", "<secondary_fields>"],
       "limit": 20
     }
   }
   ```
4. Display matching items as a table
5. Show count of results found

## Example

Input: `articles machine learning`

```json
{
  "action": "read",
  "collection": "articles",
  "query": {
    "search": "machine learning",
    "fields": ["id", "title", "status", "date_created"],
    "limit": 20
  }
}
```

</details>

### `snapshot-schema`

Get a schema snapshot — shows complete data model for one or more collections

Arguments: `collection1 collection2 ...`

<details><summary>Prompt template</summary>

# Snapshot Schema

Get a detailed view of the Directus data model.

## Arguments

- `collection1 collection2 ...` (optional) — Specific collections to snapshot. If omitted, shows discovery overview.

## Process

1. **No arguments:**
   - Call `schema` tool (discovery mode) to list all collections
   - Display collections grouped by folders
   - Show collection count and notes

2. **With collection names:**
   - Call `schema` tool with `keys: ["collection1", "collection2", ...]`
   - For each collection, display:
     - Fields table: name, type, interface, required, default, notes
     - Relations: field → target collection (type, on_delete)
     - Meta: icon, display_template, singleton, versioning, archive settings
   - Call `relations` tool with `action: "read"` for full relation details

3. Format output for easy reading — this is useful for understanding what to migrate or document.

## Example

Input: `posts authors categories`

Shows detailed schema for posts, authors, and categories including all fields, relations, and meta configuration.

</details>

### `trigger-flow`

Trigger a Directus automation flow

Arguments: `<flow-name-or-id> <collection> [--keys <id1,id2>] [--data <json>]`

<details><summary>Prompt template</summary>

# Trigger Flow

## Arguments

- `flow-name-or-id` (required) — Flow name (partial match) or UUID
- `collection` (required) — Collection context
- `--keys <id1,id2>` — Item IDs (required if flow needs selection)
- `--data <json>` — JSON payload passed as `$trigger.body`

## Process

1. **Find the flow:**
   - Call `flows` tool with `action: "read"` to list all flows
   - Match by name (partial, case-insensitive) or by UUID
   - If multiple matches, ask user to clarify

2. **Read flow definition:**
   - Call `flows` tool with `action: "read"` and `key: "<flow-uuid>"`
   - Check trigger type (must be manual or webhook for programmatic triggering)
   - Check if `requireSelection` is set (if yes, `--keys` is required)
   - Show flow details to user before triggering

3. **Trigger the flow:**
   ```json
   {
     "id": "<flow-uuid>",
     "collection": "<collection>",
     "keys": ["<id1>", "<id2>"],
     "data": { "custom": "payload" }
   }
   ```

4. Display the flow execution result

## Example

Input: `"Welcome Email" contacts --keys abc-123 --data {"send_now": true}`

## Important

- Always read the flow definition before triggering
- Verify the flow is active (status: "active")
- Check trigger type compatibility
- Warn user before triggering flows with side effects (emails, webhooks, deletions)

</details>
