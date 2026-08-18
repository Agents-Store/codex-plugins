# nocodb-ops

> NocoDB ops plugin for Agents Store. Record management, views, reports, filtering, search, and data import/export for business users via MCP tools and CLI.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/nocodb-ops

## Skills

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

- **build-report** — Build an aggregation report from NocoDB table data
- **create-record** — Create a new record in a NocoDB table
- **create-view** — Create a new view for a NocoDB table
- **list-records** — List records from a NocoDB table with optional filtering
- **list-tables** — List all tables in the NocoDB base
- **search-records** — Search records in a NocoDB table by keyword

## Subagents

Codex does not install plugin subagents automatically — copy them manually before use:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

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


## MCP servers

Add to `~/.codex/config.toml`:

```toml
[mcp_servers.nocodb]
url = "${NOCODB_MCP_URL}"
type = "http"

[mcp_servers.nocodb.headers]
"xc-mcp-token" = "${NOCODB_MCP_TOKEN}"

```
