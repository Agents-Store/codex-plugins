# postgresql-external-dev

> PostgreSQL knowledge for low-code stacks. Schema design for external database connections (compatible SQL patterns for NocoDB and NocoBase — table creation, column types, relations, indexes, anti-patterns), plus the 29-tool PostgreSQL MCP reference and the PostgREST REST API.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/postgresql-external-dev

## Skills

- **column-types** — This skill should be used when the user asks "what SQL type for", "which column type", "NocoDB column types", "NocoBase field types", "type compatibility", "how to store email/url/phone/json/rating", "what type for currency", "decimal vs double", "jsonb", "uuid column", "ARRAY column", "PostgreSQL enum", or needs to choose the correct PostgreSQL type that works in both NocoDB and NocoBase.

- **create-tables** — This skill should be used when the user asks to "create a table", "design a schema", "set up a database", "create PostgreSQL tables for NocoDB", "create tables for NocoBase external DB", "what PK type to use", "UUID primary key", "table without a primary key", "does NocoBase need a license", or needs to understand how NocoDB and NocoBase interact with the same PostgreSQL database.

- **examples** — This skill should be used when the user asks for a "full example", "complete schema", "e-commerce schema", "blog schema", "CRM schema", "content management schema", "show me a real schema", "sample database", "end-to-end example", or wants to see a complete working PostgreSQL schema with all tables, constraints, indexes, and relations that is compatible with NocoDB and NocoBase.

- **modify-schema** — This skill should be used when the user asks to "alter a table", "rename a column", "change column type", "drop a column", "drop a table", "remove a constraint", "delete a junction table", "add default value", "set NOT NULL", "sync schema", "meta sync", "refresh data source", "NocoDB doesn't show the new column", or needs to modify an existing PostgreSQL schema that is connected to NocoDB or NocoBase.

- **postgres-mcp-tools** — This skill should be used when the user wants to "run SQL through MCP", "use the PostgreSQL MCP tools", "execute_sql", "inspect a database schema over MCP", "list indexes, locks or bloated tables", "analyze query performance with MCP", "check database status", "get a query plan", "MCP Toolbox for Databases", or needs the tool reference of the 29-tool PostgreSQL MCP server for querying and administering a PostgreSQL database.
- **postgrest-api** — This skill should be used when the user wants to "use PostgREST", "make REST calls to PostgreSQL", "CRUD over HTTP on a PostgreSQL table", "upsert with on_conflict", "Prefer header options", "return=representation", "max-affected", "call a stored function over REST", "read PostgreSQL from an n8n HTTP Request node", or "read PostgreSQL from a Trigger.dev task with fetch".
- **relations** — This skill should be used when the user asks to "create a relation", "add foreign key", "set up one-to-many", "many-to-many relation", "one-to-one", "self-referential", "junction table", "composite primary key", "FK constraint", "create index on FK", or needs to connect tables in a PostgreSQL database that works with NocoDB and NocoBase.

- **troubleshoot** — This skill should be used when the user asks about "incompatible types", "what to avoid", "schema checklist", "NocoDB doesn't recognize", "NocoBase can't read", "ARRAY not working", "ENUM alternative", "jsonb support", "UUID primary key problem", "external PostgreSQL missing in NocoBase", "naming conventions", "validation checklist", or encounters issues with a PostgreSQL schema connected to NocoDB or NocoBase.


## Subagents

Codex does not install plugin subagents automatically — copy them manually before use:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- **postgresql-schema-designer** — Use this agent when the user needs help designing PostgreSQL database schemas that are compatible with low-code platforms (NocoDB, NocoBase) as external data sources.

<example>
Context: User needs to design a multi-table schema
user: "Help me design a database schema for a project management app that will be connected to NocoDB"
assistant: "I'll use the postgresql-schema-designer agent to design a compatible schema with proper relations."
<commentary>
User needs a multi-table schema with relations — agent will ensure all tables, FK constraints, indexes, and types are compatible with NocoDB and NocoBase.
</commentary>
</example>

<example>
Context: User needs to set up a Many-to-Many relation
user: "I need to connect products and tags with a many-to-many relation for my NocoBase external database"
assistant: "I'll use the postgresql-schema-designer agent to create the junction table with proper composite PK."
<commentary>
M2M relations require specific NocoDB-style junction tables with composite PK — agent knows this pattern.
</commentary>
</example>

<example>
Context: User is unsure which column type to use
user: "What PostgreSQL type should I use for a currency field that works in both NocoDB and NocoBase?"
assistant: "I'll use the postgresql-schema-designer agent to recommend the compatible type."
<commentary>
Agent has the full compatibility table and can recommend decimal(10,2) for currency fields.
</commentary>
</example>

