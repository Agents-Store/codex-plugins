# dify-dev

> Dify API dev plugin for Agents Store. Complete coverage of the Dify App Service API (chat, completion, workflows, conversations, files, audio, annotations) and the Knowledge Base / Datasets API — auth, endpoints, streaming events, curl examples, and troubleshooting.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/dify-dev

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **annotations** — This skill should be used when the user asks to "list/create/update/delete Dify annotations", "call /apps/annotations", "set up Dify annotation reply", "enable annotation reply", "disable annotation reply", or "check annotation reply status". Covers the annotation (curated Q&A) subsystem.

- **chat-completion** — This skill should be used when the user asks to "send a chat message to Dify", "call /chat-messages", "use the Dify completion API", "/completion-messages", "stream a Dify chat response", "stop Dify generation", "get suggested questions", or "submit message feedback / like a message". Covers conversational (chat/agent/ chatflow) and stateless (completion) message sending.

- **conversations** — This skill should be used when the user asks to "list Dify conversations", "get conversation history", "list messages in a Dify conversation", "rename a Dify conversation", "delete a conversation", or "get/update Dify conversation variables". Covers chat-history management for chat/agent/chatflow apps.

- **examples** — This skill should be used when the user asks for a "full Dify example", "end-to-end Dify integration", "how do I build a chatbot with Dify", "complete workflow example", "Dify RAG example", or wants a working, copy-paste walkthrough that strings multiple Dify API calls together.

- **files-audio** — This skill should be used when the user asks to "upload a file to Dify", "call /files/upload", "send an image to a Dify app", "transcribe audio with Dify", "/audio-to-text", "Dify text to speech", or "/text-to-audio". Covers file upload for multimodal input and the speech endpoints.

- **knowledge-base** — This skill should be used when the user asks to work with Dify "knowledge base", "datasets API", "create a dataset", "upload a document to Dify", "add documents to a knowledge base", "segments / chunks", "retrieve from a Dify knowledge base", "test retrieval", or RAG ingestion. Covers the standalone Knowledge Base / Datasets API.

- **setup** — This skill should be used when the user asks how to "connect to the Dify API", "authenticate with Dify", "get a Dify API key", "what is the Dify base URL", "blocking vs streaming in Dify", "the Dify user field", "send files to a Dify app", or needs the app metadata endpoints (/info, /parameters, /meta, /site). Foundation for every other Dify API call.

- **troubleshoot** — This skill should be used when a Dify API call fails or behaves unexpectedly — "Dify 401 / 404 / 429 error", "Dify conversation not found", "Dify blocking timeout", "Dify rate limit", "why is my Dify conversation empty", "Dify user mismatch", or "Dify file upload not working". Error codes, limits, and common pitfalls.

- **workflows** — This skill should be used when the user asks to "run a Dify workflow", "call /workflows/run", "execute a Dify workflow app via API", "get a workflow run result", "list Dify workflow logs", "stop a Dify workflow task", or work with Chatflow/Workflow node events. Covers Workflow-type apps (no conversation wrapper).


## Subagents

Defined under `.codex/agents/` as TOML files:

- **dify-developer** — Use this agent when the user needs help building with the Dify API — writing integration code against a Dify app, debugging API calls (chat, completion, workflow), ingesting documents into a knowledge base, or wiring Dify into their backend.

<example>
Context: User is integrating a Dify chatbot into their app
user: "Help me call my Dify chatbot from my backend and keep the conversation going across messages"
assistant: "I'll use the dify-developer agent to build the chat integration with conversation handling."
<commentary>
Developer needs chat integration code with conversation_id continuity — agent routes to the chat-completion and conversations skills.
</commentary>
</example>

<example>
Context: User is debugging a failing Dify API call
user: "My Dify /chat-messages call returns 404 and the conversation comes back empty"
assistant: "I'll use the dify-developer agent to diagnose the Dify API error."
<commentary>
Classic user-field/conversation-isolation issue — agent uses the troubleshoot skill to pinpoint it.
</commentary>
</example>

<example>
Context: User wants to populate and query a knowledge base
user: "How do I upload docs into a Dify knowledge base and test retrieval over the API?"
assistant: "I'll use the dify-developer agent to build the knowledge-base ingestion and retrieval flow."
<commentary>
Developer needs the Datasets API end to end — agent routes to the knowledge-base skill.
</commentary>
</example>


## Workflows (canonical slash commands)

Codex CLI doesn't support custom slash commands — invoke these workflows via natural language. Each entry below is a prompt template you can adapt:

### `api`

Look up a Dify API endpoint — HTTP method, path, parameters, response shape, and a ready curl example

Arguments: `[endpoint-or-keyword]`

<details><summary>Prompt template</summary>

# Dify API Lookup

Look up one or more Dify API endpoints and return a precise, copy-paste-ready answer.

## Argument

- `$ARGUMENTS` — an endpoint path or a keyword. Examples: `chat-messages`, `run a workflow`,
  `upload file`, `retrieve`, `delete conversation`, `annotation reply`, `text-to-audio`,
  `create dataset`. If empty, list the endpoint groups and ask which one.

## Process

1. **Map the query to a skill** using this routing:
   - chat / message / completion / stop / suggested / feedback → `chat-completion`
   - workflow / run / logs / node events → `workflows`
   - conversation / history / variables → `conversations`
   - file / upload / audio / speech / tts / stt → `files-audio`
   - annotation → `annotations`
   - dataset / knowledge base / document / segment / chunk / retrieve / RAG → `knowledge-base`
   - auth / base url / user field / parameters / info / meta / site → `setup`
   - error / 401 / 404 / 429 / timeout / rate limit → `troubleshoot`

2. **Invoke the matching skill** with the Skill tool to load the authoritative reference, then
   (for the knowledge-base group) read the relevant file under
   `skills/knowledge-base/references/` for full parameter tables.

3. **Answer with**, for each matched endpoint:
   - **HTTP method + path** (relative to base URL `https://api.dify.ai/v1` or `https://{host}/v1`)
   - **Auth**: app key (`app-…`) or Knowledge key (`dataset-…`)
   - **Key parameters** (required vs optional, with the `user` field noted)
   - **Response shape** (concise JSON sketch)
   - **A ready curl example** with placeholder values

4. If the query is ambiguous or matches several endpoints, list the candidates briefly and
   show the most likely one in full.

## Rules

- Use only endpoints and shapes documented in this plugin's skills — never invent paths.
- All examples are **curl**. Show both the Cloud base URL and note the self-hosted variant
  when relevant.
- Keep it tight: method/path, params, response, one curl example. No filler.

</details>

### `generate-client`

Generate copy-paste curl scripts for a Dify operation (chat, completion, workflow, file upload, knowledge-base ingest/retrieve)

Arguments: `[operation]`

<details><summary>Prompt template</summary>

# Generate Dify curl Client

Generate a ready-to-run **curl** script for a chosen Dify operation, with placeholders for
the base URL and API key.

## Argument

- `$ARGUMENTS` — optional operation hint (e.g. `chat`, `completion`, `workflow`,
  `file upload`, `kb ingest`, `kb retrieve`). If empty, ask which operation to scaffold.

## Process

1. **Determine the operation.** If `$ARGUMENTS` is empty or ambiguous, use AskUserQuestion to
   choose among: Chat message · Completion · Run workflow · Upload file → chat · Knowledge
   base ingest · Knowledge base retrieve.

2. **Confirm a few inputs** (AskUserQuestion or sensible defaults):
   - Deployment: Cloud (`https://api.dify.ai/v1`) or self-hosted (`https://{host}/v1`)
   - Response mode where applicable: `streaming` or `blocking`
   - Whether to emit a single curl command or a small multi-step `.sh` script

3. **Load the authoritative request shape** by invoking the matching skill with the Skill
   tool (`chat-completion`, `workflows`, `files-audio`, or `knowledge-base`). Use only
   documented parameters.

4. **Generate the script** using shell variables for secrets, e.g.:
   ```bash
   #!/usr/bin/env bash
   set -euo pipefail
   BASE="${DIFY_BASE_URL:-https://api.dify.ai/v1}"
   KEY="${DIFY_API_KEY:?set DIFY_API_KEY}"   # app-… (or dataset-… for knowledge base)
   USER="${DIFY_USER:-user-123}"
   # ... curl call(s) ...
   ```
   - Use the app key (`app-…`) for Service API ops, the Knowledge key (`dataset-…`) for KB ops.
   - Always include the `user` field where the endpoint requires it.
   - Add `-N` for streaming calls and a short comment on which SSE events to expect.
   - For multi-step flows (e.g. KB ingest → poll indexing → retrieve), chain the calls and
     capture ids with `jq` where helpful.

5. **Output** the script in a fenced block. If the user asked to save it, Write it to a path
   they specify (default: `./dify-<operation>.sh`) and remind them to `chmod +x`.

## Rules

- **curl only.** No language SDKs.
- Never hardcode real keys — read from env vars and show the export commands.
- Only use endpoints/parameters documented in this plugin's skills.

</details>

### `quickstart`

Guided Dify API connect — find your app API key, set the base URL, send a first test call, verify the response

Arguments: `[base-url]`

<details><summary>Prompt template</summary>

# Dify API Quickstart

Walk the user from zero to a verified first API call against their Dify app.

## Argument

- `$ARGUMENTS` — optional base URL (e.g. `https://api.dify.ai/v1` or `https://dify.example.com/v1`).
  If empty, ask whether they're on Dify Cloud or self-hosted.

## Process

### Step 1 — Base URL
Confirm the base URL. Cloud is `https://api.dify.ai/v1`; self-hosted is `https://{host}/v1`
(must end in `/v1`). If `$ARGUMENTS` was given, validate it ends with `/v1`.

### Step 2 — API key
Explain where to get the key, then ask the user to provide it (or to export it as an env var
so it isn't pasted into history):
- **App key** (`app-…`): Dify Studio → your app → **API Access** → **API Key**. Used for chat/completion/workflow.
- **Knowledge key** (`dataset-…`): Dify → **Knowledge** → **API**. Used for the Datasets API.

Recommend: `export DIFY_API_KEY=app-XXXX` and `export DIFY_BASE_URL=...`.

### Step 3 — Identify the app
Invoke the `setup` skill for context, then run `GET /info` and `GET /parameters` to learn the
app `mode` and required `inputs`:

```bash
curl -s "$DIFY_BASE_URL/info"       --header "Authorization: Bearer $DIFY_API_KEY"
curl -s "$DIFY_BASE_URL/parameters" --header "Authorization: Bearer $DIFY_API_KEY"
```

Report the app type (`chat`/`agent-chat`/`advanced-chat`/`workflow`/`completion`) and any
required input variables.

### Step 4 — First test call
Pick the call that matches the app `mode` and run it (use a stable `user`, e.g. `quickstart-user`):

- chat / agent / advanced-chat:
  ```bash
  curl -s -X POST "$DIFY_BASE_URL/chat-messages" \
    --header "Authorization: Bearer $DIFY_API_KEY" \
    --header 'Content-Type: application/json' \
    --data '{ "query": "Hello from quickstart", "inputs": {}, "response_mode": "blocking", "user": "quickstart-user" }'
  ```
- workflow: `POST /workflows/run` with the `inputs` from Step 3.
- completion: `POST /completion-messages` with the app's `inputs`.

### Step 5 — Verify & explain
- Success → show the `answer`/`outputs` and the captured `conversation_id`/`workflow_run_id`,
  and point to next skills (`chat-completion`, `workflows`, `conversations`, `knowledge-base`).
- Failure → route to the `troubleshoot` skill and map the status/`code` to a fix (401 key,
  404 user/base-url, 400 inputs, timeout → use streaming).

## Rules

- Never echo the full API key back; refer to it via the env var.
- Use only documented endpoints. All examples are curl.
- Prefer running the calls with the Bash tool only if the user has provided/exported a key;
  otherwise print the commands for them to run.

</details>
