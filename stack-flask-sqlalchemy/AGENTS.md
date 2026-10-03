# stack-flask-sqlalchemy

> Flask + SQLAlchemy architecture plugin. How the application factory, the Flask-SQLAlchemy session, Alembic migrations and Jinja2 templates fit together: where db is created, the app context, the transaction boundary, expire_on_commit, N+1 at the route-to-template edge, and a full-feature recipe. Tool knowledge comes from its dependencies flask-dev and sqlalchemy-dev.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/stack-flask-sqlalchemy

## Skills

- **full-feature** — Use when the user asks to "add a new feature", "create a new page", "build CRUD for a new entity", "add a new section to the app", "implement a full feature end-to-end", or needs a step-by-step recipe for building a complete feature across Flask + SQLAlchemy layers.

- **layers-and-boundaries** — Use when the user asks about "Flask SQLAlchemy architecture", "where to create db in Flask", "Flask app context and SQLAlchemy session", "when to commit in Flask", "Flask transaction boundary", "expire_on_commit in Flask-SQLAlchemy", "DetachedInstanceError in Flask", "Working outside of application context", "N+1 in a Jinja template", "lazy raise in Flask", "data saved in tests but lost in production", or needs the rules for where each layer of a Flask + SQLAlchemy app starts and ends.


## Subagents

Codex does not install plugin subagents automatically — copy them manually before use:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

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
Issue spans model relationships (Appointment→Client), the route's query (a missing joinedload, or a lazy='raise' error), and template rendering. Orchestrator traces the full data path.
</commentary>
</example>

<example>
Context: User wants to add authentication to existing pages
user: "I need to add login/register and make sure each user only sees their own clients"
assistant: "I'll use the stack-orchestrator agent to wire Flask-Login with the User model and add auth to all routes."
<commentary>
Auth touches every layer — User model, login routes, @login_required decorators, template conditionals, and query scoping by user_id (owned_by). The login flow itself is in flask-dev (auth-flask-login), the User model in sqlalchemy-dev.
</commentary>
</example>

