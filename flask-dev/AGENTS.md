# flask-dev

> Flask dev plugin for Agents Store. Application factory patterns, blueprint organization, Jinja2 templates, Flask CLI recipes, and troubleshooting for developers building with Flask.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/flask-dev

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **api-reference** — Use when the user asks for "Flask API reference", "Flask decorators", "Flask request object", "Flask response", "Flask config options", "Flask url_for", "Flask flash messages", or needs specific Flask framework API details.

- **app-patterns** — Use when the user asks about "Flask application factory", "Flask blueprints", "Flask config management", "Flask extensions", "organize Flask project", "Flask app structure", "register Flask blueprint", "Flask context processors", or needs patterns for structuring a Flask application.

- **cli-recipes** — Use when the user asks about "Flask CLI", "flask run", "flask shell", "flask routes", "Flask command line", "custom Flask CLI command", "run Flask from terminal", or needs ready-to-use Flask CLI commands.

- **jinja2-patterns** — Use when the user asks about "Jinja2 templates", "Flask templates", "template inheritance", "Jinja2 macros", "Jinja2 filters", "Flask render_template", "base template", "template blocks", or needs patterns for Jinja2 template engine in Flask.

- **setup** — Use when the user asks to "verify Flask project setup", "check Flask structure", "is my Flask app set up correctly", "validate Flask project", or needs to confirm that a Flask project follows recommended patterns and has correct file structure.

- **troubleshoot** — Use when the user encounters "Flask errors", "Flask not working", "Flask import error", "Flask 500 error", "debug Flask", "Flask template not found", "Flask circular import", or needs to diagnose and fix common Flask problems.


## Subagents

Defined under `.codex/agents/` as TOML files:

- **flask-developer** — Use this agent when the user needs help building with Flask — writing route handlers, organizing blueprints, designing templates, debugging Flask errors, or working with Flask extensions in their project.

<example>
Context: User is adding a new feature to a Flask app
user: "Help me create a new blueprint for managing appointments with CRUD routes"
assistant: "I'll use the flask-developer agent to build the appointments blueprint."
<commentary>
Developer needs help creating a Flask blueprint with route handlers and templates.
</commentary>
</example>

<example>
Context: User is debugging a Flask error
user: "I'm getting a circular import error when I try to import my models in a route file"
assistant: "I'll use the flask-developer agent to diagnose and fix the circular import."
<commentary>
Developer has a common Flask structural issue — agent can analyze the import chain and fix it.
</commentary>
</example>

<example>
Context: User wants to improve Flask app architecture
user: "My Flask app has all routes in one file, help me split it into blueprints"
assistant: "I'll use the flask-developer agent to refactor the app into a blueprint structure."
<commentary>
Developer needs architectural guidance for Flask project organization.
</commentary>
</example>

