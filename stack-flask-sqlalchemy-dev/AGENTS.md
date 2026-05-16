# stack-flask-sqlalchemy-dev

> Flask + SQLAlchemy stack dev plugin for Agents Store. Integration patterns for app factory wiring, blueprint-model coordination, Flask-Login + SQLAlchemy auth, Jinja2 + query data, and full-feature recipes.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/stack-flask-sqlalchemy-dev

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **auth-integration** — Use when the user asks about "Flask-Login with SQLAlchemy", "user authentication", "login registration flow", "password hashing", "protect routes", "current_user with database", "session management Flask", or needs patterns for integrating Flask-Login authentication with SQLAlchemy user models.

- **flask-sqlalchemy-wiring** — Use when the user asks about "connect Flask to SQLAlchemy", "Flask-SQLAlchemy wiring", "blueprint model integration", "pass query data to template", "form to database", "render database data in template", "Flask model view pattern", or needs patterns for connecting Flask routes, SQLAlchemy models, and Jinja2 templates.

- **full-feature** — Use when the user asks to "add a new feature", "create a new page", "build CRUD for a new entity", "add a new section to the app", "implement a full feature end-to-end", or needs a step-by-step recipe for building a complete feature across Flask + SQLAlchemy layers.

- **init-project** — Use when the user asks to "set up Flask project", "initialize Flask SQLAlchemy project", "scaffold Flask app", "create Flask project structure", "bootstrap Flask application", or needs to set up a new Flask + SQLAlchemy project from scratch with all integrations wired.


## Subagents

Defined under `.codex/agents/` as TOML files:

- **stack-orchestrator** — Use this agent when the user needs help coordinating work across Flask and SQLAlchemy layers — building features that span models, routes, and templates, debugging cross-layer issues, or planning multi-step implementations.

<example>
Context: User is building a new feature end-to-end
user: "Build a finance tracking page where I can add expenses and see monthly earnings from appointments"
assistant: "I'll use the stack-orchestrator agent to coordinate the implementation across model, routes, and templates."
<commentary>
Feature spans all layers — model (Spending), routes (finance blueprint), templates (finance.html), and queries (aggregate earnings from Appointment). Orchestrator coordinates the full implementation.
</commentary>
</example>

<example>
Context: User is debugging a cross-layer issue
user: "My appointments page shows the wrong client name — it shows 'None' instead of the actual name"
assistant: "I'll use the stack-orchestrator agent to trace the data flow from model through route to template."
<commentary>
Issue spans model relationships (Appointment→Client), route query (missing joinedload), and template rendering. Orchestrator traces the full data path.
</commentary>
</example>

<example>
Context: User wants to add authentication to existing pages
user: "I need to add login/register and make sure each user only sees their own clients"
assistant: "I'll use the stack-orchestrator agent to wire Flask-Login with the User model and add auth to all routes."
<commentary>
Auth integration touches every layer — User model, login routes, @login_required decorators, template conditionals, and query filtering by user_id.
</commentary>
</example>

