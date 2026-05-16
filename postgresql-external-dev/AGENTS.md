# postgresql-external-dev

> PostgreSQL schema design for external database connections. Compatible SQL patterns for NocoDB and NocoBase — table creation, column types, relations, indexes, and anti-patterns.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/postgresql-external-dev

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **column-types** — This skill should be used when the user asks "what SQL type for", "which column type", "NocoDB column types", "NocoBase field types", "type compatibility", "how to store email/url/phone/json/rating", "what type for currency", "decimal vs double", or needs to choose the correct PostgreSQL type that works in both NocoDB and NocoBase.

- **create-tables** — This skill should be used when the user asks to "create a table", "design a schema", "set up a database", "create PostgreSQL tables for NocoDB", "create tables for NocoBase external DB", "what PK type to use", or needs to understand how NocoDB and NocoBase interact with the same PostgreSQL database.

- **examples** — This skill should be used when the user asks for a "full example", "complete schema", "e-commerce schema", "blog schema", "CRM schema", "content management schema", "show me a real schema", "sample database", "end-to-end example", or wants to see a complete working PostgreSQL schema with all tables, constraints, indexes, and relations that is compatible with NocoDB and NocoBase.

- **modify-schema** — This skill should be used when the user asks to "alter a table", "rename a column", "change column type", "drop a column", "drop a table", "remove a constraint", "delete a junction table", "add default value", "set NOT NULL", or needs to modify an existing PostgreSQL schema that is connected to NocoDB or NocoBase.

- **relations** — This skill should be used when the user asks to "create a relation", "add foreign key", "set up one-to-many", "many-to-many relation", "one-to-one", "self-referential", "junction table", "composite primary key", "FK constraint", "create index on FK", or needs to connect tables in a PostgreSQL database that works with NocoDB and NocoBase.

- **troubleshoot** — This skill should be used when the user asks about "incompatible types", "what to avoid", "schema checklist", "NocoDB doesn't recognize", "NocoBase can't read", "ARRAY not working", "ENUM alternative", "naming conventions", "validation checklist", or encounters issues with a PostgreSQL schema connected to NocoDB or NocoBase.


## Subagents

Defined under `.codex/agents/` as TOML files:

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

