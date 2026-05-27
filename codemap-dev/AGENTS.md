# codemap-dev

> Code understanding plugin for developers. Helps onboard to unfamiliar projects through beginner-friendly code review, step-by-step explanations, and visual diagrams (architecture, ERD, flows) via drawio-mcp.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/codemap-dev

## MCP servers

Add to `~/.codex/config.toml`:

```toml
[mcp_servers.drawio]
url = "https://mcp.draw.io/mcp"
type = "http"

```

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **codemap-diagram** — This skill should be used when the user asks to "draw a diagram", "visualize architecture", "show me the database schema", "create an ERD", "sequence diagram", "flow diagram", "dependency graph", "architecture diagram", "C4 diagram", or needs any visual representation of code structure, data flow, or system architecture. All diagrams are generated as native mxGraph XML and rendered via drawio-mcp. Also triggers when a code explanation would benefit from a visual aid.

- **codemap-explain** — This skill should be used when the user asks to "explain this code", "what does this file do", "how does this work", "walk me through this function", "explain this module", "what is this for", "help me understand this", "break down this code for me", "give me a tour of this codebase", or needs a beginner-friendly step-by-step explanation of code, files, functions, or modules. Also triggers when the user points at code and asks "why" or "how" questions, or says "I don't understand this", "what's happening here", "trace this flow for me".

- **codemap-review** — This skill should be used when the user asks to "review code", "check this file", "what's wrong with this code", "review my PR", "code quality check", "find issues in this code", or wants feedback on readability, style, security, or common beginner mistakes. Provides structured review with "why" explanations, not just "what" fixes. Also triggers when a developer asks "is this code okay", "what can I improve", or "check my work".

- **codemap-examples** — This skill should be used when the user asks for "codemap examples", "how to use codemap", "show me what codemap can do", "codemap walkthrough", or wants to see end-to-end usage scenarios for the codemap plugin.


## Subagents

Defined under `.codex/agents/` as TOML files:

- **architect-explainer** — Use this agent when the user wants to understand project architecture, how components connect, what a module does, or needs a guided tour of a codebase.

<example>
Context: User just joined a project and wants to understand it
user: "Explain the architecture of this project to me"
assistant: "I'll use the architect-explainer agent to analyze and explain the project structure."
<commentary>
New developer needs a guided tour of the project architecture.
</commentary>
</example>

<example>
Context: User wants to understand a specific module
user: "How does the authentication system work in this project?"
assistant: "I'll use the architect-explainer agent to trace and explain the auth flow."
<commentary>
Developer wants to understand a specific subsystem with context.
</commentary>
</example>

<example>
Context: User wants to understand data flow
user: "How does data flow from the form to the database in this app?"
assistant: "I'll use the architect-explainer agent to trace the data flow."
<commentary>
Developer wants to understand the request lifecycle.
</commentary>
</example>

- **code-reviewer** — Use this agent when the user wants a code review with beginner-friendly explanations — reviewing files, PRs, diffs, or asking about code quality, security, or style issues.

<example>
Context: User wants feedback on a specific file
user: "Review routes/deals.py for me"
assistant: "I'll use the code-reviewer agent to analyze the file."
<commentary>
Developer wants a beginner-friendly code review of a specific file.
</commentary>
</example>

<example>
Context: User wants to understand what's wrong with their code
user: "What mistakes am I making in this file?"
assistant: "I'll use the code-reviewer agent to identify issues and explain them."
<commentary>
Beginner developer wants to learn from their mistakes with explanations.
</commentary>
</example>

<example>
Context: User wants a PR reviewed
user: "Review PR #15 and explain issues for a junior developer"
assistant: "I'll use the code-reviewer agent to review the PR diff."
<commentary>
Developer wants a PR review with educational explanations.
</commentary>
</example>

- **diagrammer** — Use this agent when the user wants to generate visual diagrams of code — architecture diagrams, ERDs, sequence diagrams, flow charts, dependency graphs, or any visual representation.

<example>
Context: User wants a database diagram
user: "Generate an ERD for this project's database"
assistant: "I'll use the diagrammer agent to analyze models and create the ERD."
<commentary>
Developer wants a visual ERD from the project's models.
</commentary>
</example>

<example>
Context: User wants an architecture overview
user: "Draw me the architecture of this application"
assistant: "I'll use the diagrammer agent to create an architecture diagram."
<commentary>
Developer wants a C4-style architecture diagram.
</commentary>
</example>

<example>
Context: User wants a flow diagram
user: "Show me the login flow as a sequence diagram"
assistant: "I'll use the diagrammer agent to trace and visualize the login flow."
<commentary>
Developer wants a sequence diagram for a specific endpoint.
</commentary>
</example>


## Workflows (canonical slash commands)

Codex CLI doesn't support custom slash commands — invoke these workflows via natural language. Each entry below is a prompt template you can adapt:

### `db`

Parse models, migrations, or schema and generate an ERD diagram + DB documentation

Arguments: `(no arguments — auto-detects models)`

<details><summary>Prompt template</summary>

# Database Schema ERD

Analyze the database schema and generate an Entity-Relationship Diagram + documentation.

## Instructions

1. Read the primary skill:
   - `${CLAUDE_PLUGIN_ROOT}/skills/codemap-diagram/SKILL.md` — ERD generation rules
2. Read ERD-specific guidance from `${CLAUDE_PLUGIN_ROOT}/skills/codemap-diagram/references/diagram-types.md`

3. Ensure output directories exist: `docs/codemap/diagrams/` — create if missing.

4. Launch the **diagrammer** agent with a prompt to:
   - Auto-detect the ORM (SQLAlchemy, Prisma, TypeORM, Django, raw SQL migrations)
   - Parse all model definitions — tables, columns, types, foreign keys, relationships
   - Review schema quality (missing indexes, naming inconsistencies, missing constraints)
   - Generate ERD as mxGraph XML → save to `docs/codemap/diagrams/erd.drawio` → call drawio-mcp
   - Generate `docs/codemap/DB.md` with table details, relationships, and quality notes

</details>

### `diagram`

Generate a specific diagram — architecture, flow, db, sequence, deps

Arguments: `<type> [scope] — types: architecture, flow <feature>, db, sequence <endpoint>, deps <module>`

<details><summary>Prompt template</summary>

# Generate Diagram

Create a visual diagram for the specified aspect of the codebase. Request: $ARGUMENTS

## Instructions

1. Read the primary skill:
   - `${CLAUDE_PLUGIN_ROOT}/skills/codemap-diagram/SKILL.md` — diagram generation rules and types

2. Parse `$ARGUMENTS` for diagram type and optional scope:
   - `architecture` → C4 container diagram of the entire system
   - `flow <feature>` → flowchart of a specific feature
   - `db` → ERD of all database tables
   - `sequence <endpoint>` → sequence diagram for a request
   - `deps <module>` → dependency graph for a module
   - If no argument or unrecognized → ask user which type they want

3. Launch the **diagrammer** agent with the diagram type, scope, and any additional context from the user's message.

The agent analyzes code, builds mxGraph XML using templates, saves .drawio files, and calls drawio-mcp for interactive preview.

</details>

### `explain`

Step-by-step explanation of a file, function, or module with mini-diagram if appropriate

Arguments: `<file-path|module-path|symbol-name>`

<details><summary>Prompt template</summary>

# Explain Code for Beginners

Provide a structured, beginner-friendly explanation. Target: $ARGUMENTS

## Instructions

1. Read the primary skill:
   - `${CLAUDE_PLUGIN_ROOT}/skills/codemap-explain/SKILL.md` — 4-layer explanation model

2. Parse `$ARGUMENTS` to determine scope:
   - If it's a file path → pass the file path to the agent
   - If it's a directory → pass the directory path to the agent
   - If it's a symbol (function/class name) → pass the symbol name to the agent
   - If no argument → ask user what to explain

3. Launch the **architect-explainer** agent with a prompt describing the target and any additional context from the user's message.

The agent applies the 4-layer model (Context → Data Flow → Details → Pitfalls), adjusts depth by scope, and generates mini-diagrams when 3+ components interact.

</details>

### `flows`

Find main user flows (entry points → services → DB) and visualize them

Arguments: `(no arguments — auto-discovers flows)`

<details><summary>Prompt template</summary>

# Discover and Visualize User Flows

Find the main user flows in the project and generate visual diagrams for each.

## Instructions

1. Read the primary skill:
   - `${CLAUDE_PLUGIN_ROOT}/skills/codemap-explain/SKILL.md` — for understanding and tracing flows

2. Ensure output directories exist: `docs/codemap/diagrams/` — create if missing.

3. Launch the **architect-explainer** agent with a prompt to:
   - Discover all route definitions / API endpoints / CLI commands
   - Group them into logical user flows (auth, CRUD, key features)
   - For each major flow (max 5-6): trace the call chain, identify decision points, note DB operations
   - Generate a flowchart diagram for each flow via the codemap-diagram skill
   - Save diagrams to `docs/codemap/diagrams/flow-{name}.drawio`
   - Write `docs/codemap/FLOWS.md` with flow descriptions and diagram links

</details>

### `onboard`

Generate a full onboarding report — README summary, stack, folder structure, entry points, how to run locally, and 3 main diagrams (architecture, main flow, DB)

Arguments: `(no arguments — analyzes current project)`

<details><summary>Prompt template</summary>

# Onboard to Current Project

Generate a comprehensive onboarding package for a developer joining this project.

## Instructions

1. Read the primary skill:
   - `${CLAUDE_PLUGIN_ROOT}/skills/codemap-explain/SKILL.md` — explanation methodology

2. Ensure output directories exist: `docs/codemap/diagrams/` — create if missing.

3. Launch the **architect-explainer** agent with a prompt to perform the full onboarding:

   **Phase 1 — Project Analysis:**
   - Read root files (README.md, CLAUDE.md, package.json/requirements.txt, docker-compose, .env.example)
   - Scan directory structure, identify tech stack, find entry points
   - Read model definitions and key configuration

   **Phase 2 — Generate Documentation:**
   - Create `docs/codemap/ONBOARDING.md` (overview, stack, directory structure, entry points, how to run, key concepts)
   - Create `docs/codemap/ARCHITECTURE.md` (components, data flow, layers, external dependencies)

   **Phase 3 — Generate 3 Diagrams** (via codemap-diagram skill):
   - `docs/codemap/diagrams/architecture.drawio` — C4 container diagram
   - `docs/codemap/diagrams/main-flow.drawio` — primary user flow
   - `docs/codemap/diagrams/erd.drawio` — database ERD

   **Phase 4 — Verification (MANDATORY):**
   - Verify all 3 diagrams were created: architecture.drawio, main-flow.drawio, erd.drawio
   - If any diagram is missing, generate it before proceeding
   - Verify all files are `.drawio` (not `.mmd` or any other format)

   **Phase 5 — Summary:**
   - List all generated files with descriptions
   - Show diagram preview URLs
   - Suggest next steps

</details>

### `review`

Review a file, directory, or PR diff in beginner-friendly mode — structured feedback with "why" explanations

Arguments: `<file-path|directory|PR#>`

<details><summary>Prompt template</summary>

# Beginner-Friendly Code Review

Review code with educational explanations. Target: $ARGUMENTS

## Instructions

1. Read the primary skill:
   - `${CLAUDE_PLUGIN_ROOT}/skills/codemap-review/SKILL.md` — review methodology and output format

2. Parse `$ARGUMENTS` to determine what to review:
   - If it's a file path → pass the file path to the agent
   - If it's a directory → pass the directory path to the agent
   - If it's a PR number (e.g., `#15` or `15`) → pass the PR number to the agent
   - If no argument → ask user what to review

3. Launch the **code-reviewer** agent with a prompt describing the target and any additional context from the user's message.

The agent applies 5-dimension analysis (Security, Correctness, Readability, Patterns, Beginner Pitfalls), generates educational findings, and uses codemap-explain/codemap-diagram skills when needed.

</details>
