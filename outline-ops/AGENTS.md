# outline-ops

> Outline knowledge-base ops plugin. Drive the full Outline REST API by curl — documents (create, search, move, archive, trash, import/export, AI answers, memberships), collections (CRUD, user/group permissions, export), comments, stars, views, shares & access requests, users & groups, attachments & file operations, revisions, templates, events (audit log), OAuth clients, and data attributes. Authenticates with a Bearer OUTLINE_API_KEY against OUTLINE_API_URL.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/outline-ops

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **api-reference** — This skill should be used when the user asks for "Outline API endpoints", "Outline REST API", "Outline curl examples", "Outline API documentation", the exact method/parameters for any Outline resource, or needs HTTP details for documents, collections, comments, stars, views, shares, access requests, auth, users, groups, attachments, file operations, revisions, templates, events, OAuth clients, or data attributes. Index into the full per-domain endpoint catalog.
- **common-operations** — This skill should be used when the user wants to do knowledge-base work in Outline — "create a document", "search Outline", "update a doc", "move a document to a collection", "create a collection", "share a document", "invite users to Outline", "star a document", "comment on a doc", or any everyday Outline operation. Provides plain-language workflows that drive the REST API and route to the exact methods.
- **examples** — This skill should be used when the user wants a worked end-to-end Outline example or walkthrough — "show me a full Outline workflow", "example of building a knowledge base in Outline", "how do I publish and share a doc via the API", "document lifecycle example", "onboard users to Outline", or wants to see several Outline API calls chained together for a real scenario.
- **setup** — This skill should be used when the user wants to "connect to Outline", "authenticate with Outline", "set up Outline access", "use my Outline API key", or before running any Outline REST API call. Establishes the API key + base URL from the environment and the global request conventions (RPC POST style, Bearer header, response envelope, limit/offset pagination, sorting, rate limits, policies).
- **troubleshoot** — This skill should be used when an Outline REST API call fails or behaves unexpectedly — "Outline returns 401 / 403 / 404 / 429 / 400", "Outline API key not working", "can't find the document", "empty data / missing results", "self-hosted Outline URL not working", "SSL error", or any Outline error response. Maps symptoms to causes and fixes.

## Subagents

Defined under `.codex/agents/` as TOML files:

- **outline-assistant** — Use this agent when the user needs help running knowledge-base operations in Outline — creating, searching, editing, moving, archiving, or sharing documents; organizing collections; managing comments, stars, templates, and revisions; inviting and permissioning users and groups; or pulling usage reports and audit logs — by driving the Outline REST API.

<example>
Context: User wants to publish a document
user: "Create a 'Q3 Roadmap' doc in the Product collection and publish it"
assistant: "I'll use the outline-assistant agent to resolve the collection and create the published document."
<commentary>
Resolve the collection by name, then documents.create with publish:true — a core Outline ops flow.
</commentary>
</example>

<example>
Context: User wants to share content externally
user: "Make a public link for our onboarding guide so a contractor can read it"
assistant: "I'll use the outline-assistant agent to find the doc, create a share, and publish the link."
<commentary>
documents.search → shares.create → shares.update {published:true} — sharing without workspace membership.
</commentary>
</example>

<example>
Context: User wants an activity report
user: "Which docs in the Handbook were updated this month and who's been editing them?"
assistant: "I'll use the outline-assistant agent to list the collection's documents and pull the audit-log events."
<commentary>
documents.list (sorted) + events.list (auditLog:true) — reporting from list and audit endpoints.
</commentary>
</example>

