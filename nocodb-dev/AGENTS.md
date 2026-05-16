# nocodb-dev

> NocoDB schema development plugin. Full Meta API v3 coverage — tables, fields (30+ types), views, filters, sorts, hooks (HookV3), comments, scripts, dashboards & widgets, workflows, plus workspaces / members / teams / tokens. Bundles both Data API and Meta API OpenAPI specs.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/nocodb-dev

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

- **api-reference** — NocoDB REST API reference for schema-development work. Loaded only on explicit cite. Use when:
- "NocoDB REST API"
- "API endpoints for tables/fields/views"
- "create a table via API"
- "what's in the OpenAPI spec"
- "Meta API endpoints"
- "field type schemas"
- "Hook v3 payload"
- "dashboard / widget API"

- **cli-reference** — NocoDB `nc` CLI reference — schema-focused commands for tables, fields, views, links, hooks. Loaded only on explicit cite. Use when:
- "nc CLI commands"
- "NocoDB CLI schema commands"
- "how do I create a table from the CLI"
- "nc field:create reference"
- "NocoDB agent-skills CLI"

- **dashboards** — Create and manage NocoDB Dashboards and Widgets via Meta API v3. Use when:
- "create a dashboard"
- "add a chart / metric / KPI widget"
- "list widgets on a dashboard"
- "fetch widget data"
- "update a dashboard"
- "delete a widget"

- **examples** — End-to-end NocoDB schema-development walkthroughs. Use when:
- "show me a schema example"
- "how do I build a CRM in NocoDB?"
- "e-commerce schema example"
- "schema design walkthrough"
- "NocoDB dev scenarios"

- **field-management** — Create, update, and delete NocoDB fields across all 30 supported types — text, numeric, date, select, attachment, JSON, geometry, links, lookup, rollup, formula, button, barcode/QR, system fields. Use when:
- "add a field"
- "create a column"
- "rename a field"
- "change field type"
- "delete a column"
- "add a formula"
- "set up lookup or rollup"
- "link two tables"

- **mcp-patterns** — NocoDB MCP tools usable for schema-development work. Use when:
- "what MCP tools can I use for schema?"
- "how do I discover NocoDB structure?"
- "MCP for nocodb-dev"
- "can MCP create tables?"
- "NocoDB MCP discovery"

- **setup** — Verify NocoDB connection for schema-development work — both transports (MCP + CLI/API). Use when:
- "check NocoDB dev setup"
- "verify NocoDB API access"
- "is the nc CLI working?"
- "can I modify schema?"
- "test NocoDB MCP connection"

- **table-management** — Create, update, rename, duplicate, and delete NocoDB tables. Use when:
- "create a new NocoDB table"
- "rename a table"
- "delete a NocoDB table"
- "set the display field"
- "duplicate a table"
- "add a table with initial fields"

- **troubleshoot** — Diagnose schema-side NocoDB errors — read-only fields, type-change rejections, broken Lookups, formula errors, view config validation, version mismatches. Use when:
- "field type change rejected"
- "Lookup not working"
- "formula returns ERR"
- "cannot delete table"
- "Kanban not grouping"
- "schema cache stale"
- "NocoDB version too old"

- **view-management** — Create, configure, and delete NocoDB views — Grid, Form, Gallery, Kanban, Calendar, Map. Use when:
- "create a kanban view"
- "add a calendar view"
- "build a form for intake"
- "make a gallery of products"
- "set up filters on a view"
- "delete a view"
- "show / hide columns on a view"

- **webhooks** — Configure NocoDB webhooks (HookV3) — triggers, conditions, and notification targets (URL, Email, Messaging, Script). Use when:
- "add a webhook"
- "fire a Slack message on insert"
- "send email when a record changes"
- "trigger n8n on update"
- "list webhooks on a table"
- "delete a hook"

- **workflows** — List, execute, and inspect NocoDB Workflows (the platform's built-in automation engine) via Meta API v3. Use when:
- "list NocoDB workflows"
- "execute a workflow"
- "view workflow execution"
- "trigger workflow on demand"
- "fetch execution results"


## Subagents

Defined under `.codex/agents/` as TOML files:

- **schema-architect** — Use this agent when the user needs to create or modify NocoDB schema — add tables, change field types, set up relations (link / lookup / rollup), build views, or wire webhooks.

<example>
Context: User wants to add a related table
user: "Add an Orders table linked to Customers, and put a Total field on it"
assistant: "I'll use the schema-architect agent to design and apply the schema change."
<commentary>
Cross-table relation work — agent discovers via MCP, plans the change, applies via CLI/API, and verifies.
</commentary>
</example>

<example>
Context: User wants a Formula field
user: "Add a 'Days Open' formula on the Tickets table that subtracts CreatedAt from now"
assistant: "I'll use the schema-architect agent to add the Formula field."
<commentary>
Computed-field work — agent picks the right field type and tests the formula on real records.
</commentary>
</example>

<example>
Context: User wants a webhook
user: "Trigger a Slack message every time a high-priority bug is created"
assistant: "I'll use the schema-architect agent to configure the webhook."
<commentary>
HookV3 with condition + Messaging notification — agent uses the webhooks skill.
</commentary>
</example>


## Workflows (canonical slash commands)

Codex CLI doesn't support custom slash commands — invoke these workflows via natural language. Each entry below is a prompt template you can adapt:

### `add-relation`

Set up a Link between two NocoDB tables, optionally with a Lookup

Arguments: `[from-table] [to-table] [cardinality]`

<details><summary>Prompt template</summary>

# Add Relation

Connect two NocoDB tables with a Link field. Optionally add a Lookup so the linked record's display name is visible on the other side.

## Steps

1. Resolve both table IDs via `mcp__nocodb__getTablesList` or `nc table:list`.
2. Decide the cardinality:
   - **bt** (belongs-to) — many-to-one. Each row on the "from" side links to one row on the "to" side. (Order belongs to Customer.)
   - **hm** (has-many) — one-to-many. Each row on the "from" side links to many rows on the "to" side. (Customer has-many Orders.)
   - **mm** (many-to-many) — both sides link to many. (Tags ↔ Articles.)
3. Snapshot both tables before:
   ```
   mcp__nocodb__getTableSchema  tableId: <fromTableId>
   mcp__nocodb__getTableSchema  tableId: <toTableId>
   ```
4. Create the link field on the "from" table:
   ```bash
   nc field:create <baseId> <fromTableId> '{
     "title": "<linkName>",
     "type":  "Links",
     "linked_table_id": "<toTableId>",
     "type_of_relation": "<bt|hm|mm>"
   }'
   ```
5. Verify the inverse link auto-appeared on the "to" table:
   ```
   mcp__nocodb__getTableSchema  tableId: <toTableId>
   ```
6. (Optional) Ask the user if they want a Lookup that surfaces a column from the linked side. If yes:
   - Find the link's column ID on the "from" side (from step 5's snapshot).
   - Find the column ID to look up on the "to" side.
   - Run:
     ```bash
     nc field:create <baseId> <fromTableId> '{
       "title": "<lookupTitle>",
       "type":  "Lookup",
       "fk_relation_column_id": "<fromLinkColumnId>",
       "fk_lookup_column_id":   "<toColumnId>"
     }'
     ```
7. Verify both fields appear in the schema, and (optionally) link one record to confirm.

## Reference

- `Skill nocodb-dev:field-management` — workflow + payloads
- `Skill nocodb-dev:cli-reference` — `nc field:create` syntax
- `Skill nocodb-dev:examples` — CRM and e-commerce relation walkthroughs (`references/scenarios/`)

## Watch Out

- For `mm`, NocoDB creates a hidden join table. If you need per-relation metadata (quantity, line price), use an explicit join table (see `examples/references/scenarios/ecommerce-relations.md`).
- Don't try to PATCH a Lookup config to point at a not-yet-existing link — NocoDB rejects with 400.

</details>

### `add-webhook`

Configure a NocoDB webhook (HookV3) on a table

Arguments: `[table-name-or-id] [event] [destination]`

<details><summary>Prompt template</summary>

# Add Webhook

Configure a HookV3 webhook on a table — fire on insert / update / delete (or bulk variants), evaluate an optional condition, dispatch to URL / Email / Messaging / Script.

## Steps

1. Resolve the target table ID via `mcp__nocodb__getTablesList` or `nc table:list`.
2. Gather the user's intent:
   - **Event**: `record` (default — fires on the chosen `operation`(s) after commit) or `manual` (fires only when explicitly invoked from a Button or Script)
   - **Operations**: array of `insert` / `update` / `delete` (one hook can listen to multiple)
   - **Destination**: URL endpoint / Email / Slack-style messaging / Script
   - **Trigger fields** (optional, for `update`): list of column IDs — fire only when one of these changed
   - **Conditional firing**: there is no top-level `condition` in v3 — gate inside the destination body using `{{record.<Field>}}` or use a `Script` notification
3. Build the payload — see **webhooks** skill for full HookV3Create shape.
4. Snapshot existing hooks first to avoid duplicates:
   ```bash
   nc hook:list <baseId> <tableId>
   ```
5. Run:
   ```bash
   nc hook:create <baseId> <tableId> '<HookV3Create JSON>'
   ```
6. Verify the hook appears in `nc hook:list`. If safe, fire a single test record matching the condition; confirm the destination receives the payload.

## Templating

Body content can reference field values via `{{record.<FieldName>}}`. Field names are case-sensitive and must match the column title exactly.

For bulk operations, the destination receives `records` (array) instead of `record` (single).

## Reference

- `Skill nocodb-dev:webhooks` — full schema + worked examples
- `Skill nocodb-dev:api-reference` — `HookV3Create` schema in `references/nocodb-openapi.json`
- `Skill nocodb-dev:cli-reference` — `nc hook:*` syntax

## Watch Out

- All v3 hooks are async-after-commit — there's no `before`/`after` distinction. NocoDB retries on non-2xx responses, so destinations must tolerate replays.
- The Email notification type requires NocoDB's SMTP plugin to be configured; otherwise emails silently fail.
- For URL hooks, prefer Authorization headers over baking secrets into the body.
- v3 has no top-level `condition` filter on hooks — gate inside the destination, or use a `Script` notification.

</details>

### `create-field`

Add a field of any of the 30 supported types to a NocoDB table

Arguments: `[table-name-or-id] [field-title] [field-type]`

<details><summary>Prompt template</summary>

# Create NocoDB Field

Add a field to an existing table. Choose any of the 30 supported types — see the **field-management** skill and `api-reference/references/field-types.md` for per-type payloads.

## Steps

1. Resolve the target table ID — `nc table:list <baseId>` if name was passed, or prompt.
2. Snapshot existing columns:
   ```
   mcp__nocodb__getTableSchema  tableId: <tableId>
   ```
3. Confirm the new field title isn't already taken.
4. Determine the field type. Common pairings:
   - "phone" → `PhoneNumber`
   - "money / price / amount" → `Currency`
   - "status / stage" → `SingleSelect` (gather options)
   - "tags / categories (many)" → `MultiSelect` (gather options)
   - "image / file / attachment" → `Attachment`
   - "linked record" → `Links` (ask cardinality: bt / hm / mm)
   - "computed value" → `Formula` (gather expression)
   - "auto-incrementing id" → `Number` with `cdf` default and unique constraint
5. Build the payload (per `field-types.md`).
6. Run `nc field:create <baseId> <tableId> '<JSON>'`.
7. Verify with `mcp__nocodb__getTableSchema` and `mcp__nocodb__queryRecords` (small page, sanity-check render).

## Reference

- `Skill nocodb-dev:field-management` — workflow + cheatsheet
- `Skill nocodb-dev:api-reference` — full type catalog (references/field-types.md)
- `Skill nocodb-dev:cli-reference` — `nc field:create` syntax

## Special Cases

- **Lookup / Rollup**: the underlying link field must already exist. If it doesn't, run `/nocodb-dev:add-relation` first.
- **Formula**: after creating, query 3 records to spot-check that the formula renders without `ERR`.
- **System fields** (`CreatedTime`, `LastModifiedTime`, `CreatedBy`, `LastModifiedBy`): created instantly with `{"title":"X","type":"<TypeName>"}`; populate themselves.

</details>

### `create-table`

Create a new NocoDB table with optional initial fields

Arguments: `[base-name-or-id] [table-title]`

<details><summary>Prompt template</summary>

# Create NocoDB Table

Create a new table in a NocoDB base. Prompt for any missing details (base, title, initial fields).

## Steps

1. Resolve the base ID from the user's input. If the base name was passed, run `nc base:list` to find the matching ID. Otherwise prompt.
2. Confirm the table title isn't already in use:
   ```
   mcp__nocodb__getTablesList
   ```
3. Ask the user for any initial fields they want included, or proceed with just a title and let them add fields later.
4. Run `nc table:create <baseId> '<JSON>'` with the gathered payload — see the **table-management** skill for the payload shape and **field-management** skill for per-type field options.
5. Verify with `mcp__nocodb__getTableSchema` and report the new `tableId`.

## Reference

Invoke the relevant skills:

- `Skill nocodb-dev:table-management` — workflow and payload shape
- `Skill nocodb-dev:field-management` — for any initial fields
- `Skill nocodb-dev:cli-reference` — exact `nc` syntax

## Confirmation

Before running the create, print the planned payload and ask the user to confirm.

</details>

### `create-view`

Create a Grid / Form / Gallery / Kanban / Calendar / Map view

Arguments: `[table-name-or-id] [view-type] [view-title]`

<details><summary>Prompt template</summary>

# Create NocoDB View

Create a new view on an existing table. Each view type has different prerequisites — see the **view-management** skill.

## Steps

1. Resolve the target table ID.
2. Snapshot existing views and the field list:
   ```
   mcp__nocodb__getTableSchema  tableId: <tableId>
   ```
3. Confirm the view title isn't already used on this table.
4. Pick the view type and validate prerequisites:
   - **Grid / Form** — no prerequisites
   - **Gallery** — needs an Attachment field for the cover image (recommended)
   - **Kanban** — needs a SingleSelect field; capture its column ID for `fk_grp_col_id`
   - **Calendar** — needs a Date or DateTime field; capture for `calendar_range[].fk_from_column_id`
   - **Map** — needs a Geometry field; capture for `fk_geo_data_col_id`
5. Run `nc view:create:<type> <baseId> <tableId> '<JSON>'` per **cli-reference**.
6. Verify with `mcp__nocodb__getTableSchema` — the new view appears in `views`.

## Reference

- `Skill nocodb-dev:view-management` — full workflow + payload shape per type
- `Skill nocodb-dev:cli-reference` — `nc view:create:*` syntax

## Notes

- View create / update / delete requires NocoDB Enterprise (self-hosted or cloud).
- After creating a view, you can add filters and sorts via `nc filter:create <viewId> '...'` and `nc sort:create <viewId> '...'`.

</details>

### `list-fields`

List all fields on a NocoDB table with their types

Arguments: `table-name-or-id`

<details><summary>Prompt template</summary>

# List Fields

Print every field on a table — title, type, options summary.

## Steps

1. Resolve the target table ID — `nc table:list <baseId>` if name was passed.
2. Run:
   ```
   mcp__nocodb__getTableSchema  tableId: <tableId>
   ```
3. Format the response as a table:

| Title | Type | Options |
|-------|------|---------|
| ... | ... | ... |

   For SingleSelect / MultiSelect, show the option titles.
   For Lookup / Rollup, show the linked column.
   For Formula, show the expression.
4. Also list views (name + type) at the bottom.

## Alternative — pure CLI

```bash
nc field:list <baseId> <tableId>
```

Returns raw JSON — useful when the user wants column IDs for scripting.

## Reference

- `Skill nocodb-dev:cli-reference` — `nc field:list` syntax
- `Skill nocodb-dev:mcp-patterns` — `getTableSchema` parameters

</details>
