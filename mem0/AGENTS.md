# mem0

> Mem0 memory management plugin. Store, search, update, and organize memories with semantic search, batch operations, file attachments, and change history tracking via MCP tools.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/mem0

## Skills

- **examples** — Tool call patterns, end-to-end workflow examples, and scenario references. This skill should be used when the user needs reference implementations, complete examples, or tool call patterns.
- **file-management** — File management — attach files to memories and search file content via vector search. This skill should be used when the user asks to upload documents, attach files, or search within attached files.
- **history-tracking** — Memory history and change tracking — view evolution of memories over time, audit modifications, and track knowledge changes. This skill should be used when the user asks to see memory changes, audit modifications, or track how information evolved.
- **memory-crud** — Memory CRUD operations — add, get, update, delete memories, and batch operations. This skill should be used when the user asks to create, read, update, or delete memories, or perform bulk memory management.
- **search-retrieval** — Search and retrieval — semantic search, listing, filtering, and relevance tuning. This skill should be used when the user asks to find memories, search knowledge, list stored information, or tune search results.
- **add-memory** — Add a new memory from text
- **attach-files** — Attach files to an existing memory
- **batch-delete** — Batch delete multiple memories at once (up to 100)
- **batch-update** — Batch update multiple memories at once (up to 100)
- **delete-memory** — Delete a memory by its ID
- **get-memory** — Get a specific memory by its ID
- **list-memories** — List all stored memories with optional pagination
- **memory-history** — View the change history of a memory
- **search-files** — Search attached files via vector search
- **search-memories** — Search memories by semantic query
- **update-memory** — Update an existing memory's content

## Subagents

Codex does not install plugin subagents automatically — copy them manually before use:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- **mem0-assistant** — Interactive memory management assistant. Helps with storing, searching, updating, and organizing memories, file attachments, and knowledge retrieval.

<example>
user: "Save a memory that the project deadline is March 30th"
</example>
<example>
user: "Search my memories for anything about API keys"
</example>
<example>
user: "Show me the history of changes for this memory"
</example>

- **mem0-knowledge-manager** — Specialized knowledge base manager. Builds and maintains structured knowledge bases using Mem0 memories, file attachments, and semantic search. Use for organizing project knowledge, team context, and documentation.

<example>
user: "Build a knowledge base from these project documents"
</example>
<example>
user: "Organize my memories about the authentication system"
</example>
<example>
user: "Audit and clean up outdated memories"
</example>


## MCP servers

Add to `~/.codex/config.toml`:

```toml
[mcp_servers.mem0]
url = "https://my-mcp.mcpware.net/mcp/mem0-user"
type = "http"

```
