# mem0

> Mem0 memory management plugin. Store, search, update, and organize memories with semantic search, batch operations, file attachments, and change history tracking via MCP tools.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/mem0

## MCP servers

Add to `~/.codex/config.toml`:

```toml
[mcp_servers.mem0]
url = "https://my-mcp.mcpware.net/mcp/mem0-user"
type = "http"

```

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **examples** — Tool call patterns, end-to-end workflow examples, and scenario references. This skill should be used when the user needs reference implementations, complete examples, or tool call patterns.
- **file-management** — File management — attach files to memories and search file content via vector search. This skill should be used when the user asks to upload documents, attach files, or search within attached files.
- **history-tracking** — Memory history and change tracking — view evolution of memories over time, audit modifications, and track knowledge changes. This skill should be used when the user asks to see memory changes, audit modifications, or track how information evolved.
- **memory-crud** — Memory CRUD operations — add, get, update, delete memories, and batch operations. This skill should be used when the user asks to create, read, update, or delete memories, or perform bulk memory management.
- **search-retrieval** — Search and retrieval — semantic search, listing, filtering, and relevance tuning. This skill should be used when the user asks to find memories, search knowledge, list stored information, or tune search results.

## Subagents

Defined under `.codex/agents/` as TOML files:

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


## Workflows (canonical slash commands)

Codex CLI doesn't support custom slash commands — invoke these workflows via natural language. Each entry below is a prompt template you can adapt:

### `add-memory`

Add a new memory from text

Arguments: `"<text to remember>" [--user <user_id>]`

<details><summary>Prompt template</summary>

# Add Memory

Store new information as a memory in Mem0.

## Process

1. **Parse the input text** into messages format:
   ```
   add_memories({
     messages: [{"role": "user", "content": "<text>"}],
     user_id: "<user_id if provided>",
     infer: true
   })
   ```

2. **Display result** with the created memory ID.

3. **Verify storage** (optional):
   ```
   search_memories({ query: "<text snippet>", user_id: "<user_id>" })
   ```

## Example Usage
```
/add-memory The project uses PostgreSQL 15 with pgvector extension
/add-memory Meeting notes: decided to migrate to microservices --user project-alpha
```

</details>

### `attach-files`

Attach files to an existing memory

Arguments: `<memory_id> --user <user_id> --files <file1,file2,...>`

<details><summary>Prompt template</summary>

# Attach Files

Attach files to an existing memory for vector search.

## Process

1. **Verify memory exists:**
   ```
   get_memory({ memory_id: "<memory_id>" })
   ```

2. **Attach files:**
   ```
   attach_files_to_memory({
     memory_id: "<memory_id>",
     user_id: "<user_id>",
     files: [
       { "filename": "<file1>", "content": "<content>" }
     ]
   })
   ```

3. **Confirm attachment** with file count.

## Example Usage
```
/attach-files abc123 --user 456 --files design-doc.md,api-spec.yaml
```

</details>

### `batch-delete`

Batch delete multiple memories at once (up to 100)

Arguments: `<memory_id1> <memory_id2> ...`

<details><summary>Prompt template</summary>

# Batch Delete

Delete multiple memories in a single operation (max 100).

## Process

1. **Parse memory IDs** from arguments.

2. **Confirm with user** before deleting.

3. **Execute batch delete:**
   ```
   batch_delete_memories({
     memory_ids: ["<id1>", "<id2>", "<id3>"]
   })
   ```

4. **Report results** with count of deleted memories.

## Example Usage
```
/batch-delete abc123 def456 ghi789
```

</details>

### `batch-update`

Batch update multiple memories at once (up to 100)

Arguments: `<memory_id1>="<text>" <memory_id2>="<text>" ...`

<details><summary>Prompt template</summary>

# Batch Update

Update multiple memories in a single operation (max 100).

## Process

1. **Parse memory ID and content pairs.**

2. **Execute batch update:**
   ```
   batch_update_memories({
     memories: [
       { "memory_id": "<id1>", "data": "<new content 1>" },
       { "memory_id": "<id2>", "data": "<new content 2>" }
     ]
   })
   ```

3. **Report results** with count of updated memories.

## Example Usage
```
/batch-update id1="Updated fact 1" id2="Updated fact 2"
```

</details>

### `delete-memory`

Delete a memory by its ID

Arguments: `<memory_id>`

<details><summary>Prompt template</summary>

# Delete Memory

Delete a specific memory permanently.

## Process

1. **Show memory to be deleted:**
   ```
   get_memory({ memory_id: "<memory_id>" })
   ```

2. **Confirm with user** before deleting.

3. **Delete memory:**
   ```
   delete_memory({ memory_id: "<memory_id>" })
   ```

## Example Usage
```
/delete-memory abc123-def456
```

</details>

### `get-memory`

Get a specific memory by its ID

Arguments: `<memory_id>`

<details><summary>Prompt template</summary>

# Get Memory

Retrieve a specific memory by its ID.

## Process

1. **Get memory:**
   ```
   get_memory({ memory_id: "<memory_id>" })
   ```

2. **Display full memory** details including content, metadata, and timestamps.

## Example Usage
```
/get-memory abc123-def456
```

</details>

### `list-memories`

List all stored memories with optional pagination

Arguments: `[--user <user_id>] [--limit <n>] [--page <n>]`

<details><summary>Prompt template</summary>

# List Memories

List all stored memories with pagination support.

## Process

1. **Get all memories:**
   ```
   get_all_memories({
     user_id: "<user_id if provided>",
     limit: <limit or 20>,
     page: <page or 1>,
     page_size: <limit or 20>
   })
   ```

2. **Display results** in a table with ID, content summary, and creation date.

## Example Usage
```
/list-memories
/list-memories --user project-alpha --limit 50
/list-memories --page 2
```

</details>

### `memory-history`

View the change history of a memory

Arguments: `<memory_id>`

<details><summary>Prompt template</summary>

# Memory History

View the complete change history of a memory over time.

## Process

1. **Get current state:**
   ```
   get_memory({ memory_id: "<memory_id>" })
   ```

2. **Get change history:**
   ```
   get_memory_history({ memory_id: "<memory_id>" })
   ```

3. **Display timeline** of changes with timestamps and old/new values.

## Example Usage
```
/memory-history abc123-def456
```

</details>

### `search-files`

Search attached files via vector search

Arguments: `"<query>" --user <user_id> [--file <filename>] [--limit <n>]`

<details><summary>Prompt template</summary>

# Search Files

Search through attached files using vector search.

## Process

1. **Run file search:**
   ```
   get_memory_files({
     user_id: <user_id>,
     search_query: "<query>",
     file_name: "<filename if provided>",
     limit: <limit or 5>
   })
   ```

2. **Display matching results** with file names and relevant content snippets.

## Example Usage
```
/search-files "authentication flow" --user 123
/search-files "database schema" --user 123 --file architecture.pdf --limit 10
```

</details>

### `search-memories`

Search memories by semantic query

Arguments: `"<query>" [--user <user_id>] [--limit <n>]`

<details><summary>Prompt template</summary>

# Search Memories

Find memories using semantic search.

## Process

1. **Run semantic search:**
   ```
   search_memories({
     query: "<search query>",
     user_id: "<user_id if provided>",
     top_k: <limit or 10>
   })
   ```

2. **Display results** in a table with ID, content snippet, and relevance score.

## Example Usage
```
/search-memories API authentication
/search-memories database schema --user project-alpha --limit 5
```

</details>

### `update-memory`

Update an existing memory's content

Arguments: `<memory_id> "<new content>"`

<details><summary>Prompt template</summary>

# Update Memory

Update the content of an existing memory.

## Process

1. **Show current content** (optional):
   ```
   get_memory({ memory_id: "<memory_id>" })
   ```

2. **Update memory:**
   ```
   update_memory({
     memory_id: "<memory_id>",
     data: "<new content>"
   })
   ```

3. **Confirm update** by showing the updated memory.

## Example Usage
```
/update-memory abc123-def456 "Updated: project now uses PostgreSQL 16"
```

</details>
