# dify-dev

> Dify API dev plugin for Agents Store. Complete coverage of the Dify App Service API (chat, completion, workflows, conversations, files, audio, annotations) and the Knowledge Base / Datasets API — auth, endpoints, streaming events, curl examples, and troubleshooting.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/dify-dev

## Skills

- **annotations** — This skill should be used when the user asks to "list/create/update/delete Dify annotations", "call /apps/annotations", "set up Dify annotation reply", "enable annotation reply", "disable annotation reply", or "check annotation reply status". Covers the annotation (curated Q&A) subsystem.

- **chat-completion** — This skill should be used when the user asks to "send a chat message to Dify", "call /chat-messages", "use the Dify completion API", "/completion-messages", "stream a Dify chat response", "stop Dify generation", "get suggested questions", or "submit message feedback / like a message". Covers conversational (chat/agent/ chatflow) and stateless (completion) message sending.

- **conversations** — This skill should be used when the user asks to "list Dify conversations", "get conversation history", "list messages in a Dify conversation", "rename a Dify conversation", "delete a conversation", or "get/update Dify conversation variables". Covers chat-history management for chat/agent/chatflow apps.

- **examples** — This skill should be used when the user asks for a "full Dify example", "end-to-end Dify integration", "how do I build a chatbot with Dify", "complete workflow example", "Dify RAG example", or wants a working, copy-paste walkthrough that strings multiple Dify API calls together.

- **files-audio** — This skill should be used when the user asks to "upload a file to Dify", "call /files/upload", "send an image to a Dify app", "transcribe audio with Dify", "/audio-to-text", "Dify text to speech", or "/text-to-audio". Covers file upload for multimodal input and the speech endpoints.

- **knowledge-base** — This skill should be used when the user asks to work with Dify "knowledge base", "datasets API", "create a dataset", "upload a document to Dify", "add documents to a knowledge base", "segments / chunks", "retrieve from a Dify knowledge base", "test retrieval", or RAG ingestion. Covers the standalone Knowledge Base / Datasets API.

- **setup** — This skill should be used when the user asks how to "connect to the Dify API", "authenticate with Dify", "get a Dify API key", "what is the Dify base URL", "blocking vs streaming in Dify", "the Dify user field", "send files to a Dify app", or needs the app metadata endpoints (/info, /parameters, /meta, /site). Foundation for every other Dify API call.

- **troubleshoot** — This skill should be used when a Dify API call fails or behaves unexpectedly — "Dify 401 / 404 / 429 error", "Dify conversation not found", "Dify blocking timeout", "Dify rate limit", "why is my Dify conversation empty", "Dify user mismatch", or "Dify file upload not working". Error codes, limits, and common pitfalls.

- **workflows** — This skill should be used when the user asks to "run a Dify workflow", "call /workflows/run", "execute a Dify workflow app via API", "get a workflow run result", "list Dify workflow logs", "stop a Dify workflow task", or work with Chatflow/Workflow node events. Covers Workflow-type apps (no conversation wrapper).

- **api** — Look up a Dify API endpoint — HTTP method, path, parameters, response shape, and a ready curl example
- **generate-client** — Generate copy-paste curl scripts for a Dify operation (chat, completion, workflow, file upload, knowledge-base ingest/retrieve)
- **quickstart** — Guided Dify API connect — find your app API key, set the base URL, send a first test call, verify the response

## Subagents

Codex does not install plugin subagents automatically — copy them manually before use:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

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

