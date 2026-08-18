# directus-dev

> Directus development plugin. Knowledge base for working with Directus MCP tools (12 tools), REST API, and @directus/sdk. Covers collections, items, fields, relations, files, flows, operations, and schema design.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/directus-dev

## Skills

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
- **create-collection** — Create a new Directus collection with optional fields
- **create-item** — Create a new item in a Directus collection
- **explore-schema** — Explore the Directus schema — list all collections or get detailed field info for a specific collection
- **list-collections** — List all collections in the Directus instance
- **list-files** — List files in Directus with optional folder or type filter
- **list-flows** — List all automation flows in Directus
- **list-items** — List items from a Directus collection with optional filters
- **search-items** — Search items across a Directus collection using full-text search
- **snapshot-schema** — Get a schema snapshot — shows complete data model for one or more collections
- **trigger-flow** — Trigger a Directus automation flow

## Subagents

Codex does not install plugin subagents automatically — copy them manually before use:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

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

