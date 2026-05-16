# sqlalchemy-dev

> SQLAlchemy dev plugin for Agents Store. Model definition patterns, relationship mapping, query optimization, Alembic migrations, and troubleshooting for developers building with SQLAlchemy 2.0+.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/sqlalchemy-dev

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **api-reference** — Use when the user asks for "SQLAlchemy API reference", "SQLAlchemy Column types", "SQLAlchemy session methods", "db.session API", "SQLAlchemy relationship options", or needs specific SQLAlchemy framework API details.

- **cli-recipes** — Use when the user asks about "Alembic commands", "database migrations", "flask db migrate", "flask db upgrade", "create migration", "rollback migration", "SQLAlchemy CLI", or needs ready-to-use database migration commands.

- **model-patterns** — Use when the user asks about "SQLAlchemy models", "define database model", "SQLAlchemy relationships", "one-to-many relationship", "many-to-many", "SQLAlchemy column types", "model constraints", "Flask-SQLAlchemy model", or needs patterns for defining database models with SQLAlchemy.

- **query-patterns** — Use when the user asks about "SQLAlchemy queries", "filter records", "SQLAlchemy select", "join tables", "aggregate query", "order by", "pagination", "N+1 query problem", "eager loading", "SQLAlchemy session", or needs patterns for querying data with SQLAlchemy.

- **setup** — Use when the user asks to "verify SQLAlchemy setup", "check database connection", "is SQLAlchemy configured correctly", "test database setup", or needs to confirm that SQLAlchemy is properly initialized in their Python project.

- **troubleshoot** — Use when the user encounters "SQLAlchemy errors", "database error", "OperationalError", "IntegrityError", "DetachedInstanceError", "SQLAlchemy not working", "migration error", "debug SQLAlchemy", or needs to diagnose and fix common SQLAlchemy problems.


## Subagents

Defined under `.codex/agents/` as TOML files:

- **sqlalchemy-developer** — Use this agent when the user needs help building with SQLAlchemy — defining models, writing queries, setting up migrations, debugging database errors, or optimizing query performance.

<example>
Context: User is defining a new database model
user: "Help me create a model for tracking appointments with relationships to clients and users"
assistant: "I'll use the sqlalchemy-developer agent to design the model with proper relationships."
<commentary>
Developer needs help defining SQLAlchemy models with foreign keys and relationships.
</commentary>
</example>

<example>
Context: User is debugging a database error
user: "I'm getting an IntegrityError when trying to delete a client who has appointments"
assistant: "I'll use the sqlalchemy-developer agent to diagnose the constraint error."
<commentary>
Developer has a foreign key constraint issue — agent can analyze the model relationships and fix it.
</commentary>
</example>

<example>
Context: User wants to optimize slow database queries
user: "My appointments page loads slowly because it makes too many database queries"
assistant: "I'll use the sqlalchemy-developer agent to identify and fix the N+1 query problem."
<commentary>
Developer needs query optimization — agent can add eager loading to fix N+1 issues.
</commentary>
</example>

