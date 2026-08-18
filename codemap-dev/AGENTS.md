# codemap-dev

> Code understanding plugin for developers. Helps onboard to unfamiliar projects through beginner-friendly code review, step-by-step explanations, visual diagrams (architecture, ERD, flows) via drawio-mcp, and frontend testing via Playwright MCP.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/codemap-dev

## Skills

- **codemap-diagram** — This skill should be used when the user asks to "draw a diagram", "visualize architecture", "show me the database schema", "create an ERD", "sequence diagram", "flow diagram", "dependency graph", "architecture diagram", "C4 diagram", or needs any visual representation of code structure, data flow, or system architecture. All diagrams are generated as native mxGraph XML and rendered via drawio-mcp. Also triggers when a code explanation would benefit from a visual aid.

- **codemap-explain** — This skill should be used when the user asks to "explain this code", "what does this file do", "how does this work", "walk me through this function", "explain this module", "what is this for", "help me understand this", "break down this code for me", "give me a tour of this codebase", or needs a beginner-friendly step-by-step explanation of code, files, functions, or modules. Also triggers when the user points at code and asks "why" or "how" questions, or says "I don't understand this", "what's happening here", "trace this flow for me".

- **codemap-review** — This skill should be used when the user asks to "review code", "check this file", "what's wrong with this code", "review my PR", "code quality check", "find issues in this code", or wants feedback on readability, style, security, or common beginner mistakes. Provides structured review with "why" explanations, not just "what" fixes. Also triggers when a developer asks "is this code okay", "what can I improve", or "check my work".

- **codemap-examples** — This skill should be used when the user asks for "codemap examples", "how to use codemap", "show me what codemap can do", "codemap walkthrough", or wants to see end-to-end usage scenarios for the codemap plugin.

- **frontend-test** — This skill should be used when the user asks to "test the frontend", "check the UI", "explore the app in a browser", "test my admin panel", "find frontend bugs", "check for console errors", "verify the app works in a browser", "test user flows", "check the website", or wants a comprehensive frontend health report for a running application. Also triggers when the user says "open my app and test it", "browse my app", "check if the UI is working", or "give me a frontend report". Uses Playwright MCP to navigate, interact, and diagnose.

- **db** — Parse models, migrations, or schema and generate an ERD diagram + DB documentation
- **diagram** — Generate a specific diagram — architecture, flow, db, sequence, deps
- **explain** — Step-by-step explanation of a file, function, or module with mini-diagram if appropriate
- **flows** — Find main user flows (entry points → services → DB) and visualize them
- **onboard** — Generate a full onboarding report — README summary, stack, folder structure, entry points, how to run locally, and 3 main diagrams (architecture, main flow, DB)
- **review** — Review a file, directory, or PR diff in beginner-friendly mode — structured feedback with "why" explanations
- **test-frontend** — Test a running app's frontend — navigate pages, check UI, find errors, generate report

## Subagents

Codex does not install plugin subagents automatically — copy them manually before use:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

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

- **frontend-tester** — Use this agent when the user wants to test the frontend of a running application — navigate pages, check UI elements, test forms, find console errors, verify user flows, or generate a frontend health report.

<example>
Context: User wants to verify their app's frontend works
user: "Test the frontend of my app running on localhost:3000"
assistant: "I'll use the frontend-tester agent to navigate and test the app."
<commentary>
Developer wants automated frontend exploration and testing of their running app.
</commentary>
</example>

<example>
Context: User wants to check their admin panel
user: "Check the admin panel at localhost:8080/admin for UI issues"
assistant: "I'll use the frontend-tester agent to explore and test the admin panel."
<commentary>
Developer wants to verify an admin interface for broken elements and errors.
</commentary>
</example>

<example>
Context: User wants a comprehensive frontend report
user: "Give me a full report on how the frontend works and any issues"
assistant: "I'll use the frontend-tester agent to do a thorough frontend analysis."
<commentary>
Developer wants a structured report covering pages, flows, errors, and UI health.
</commentary>
</example>


## MCP servers

Add to `~/.codex/config.toml`:

```toml
[mcp_servers.drawio]
url = "https://mcp.draw.io/mcp"
type = "http"

[mcp_servers.playwright]
command = "npx"
args = ["@playwright/mcp@latest","--headless"]

```
