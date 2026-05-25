# taiga-ops

> Taiga project-management ops plugin. Drive the full Taiga REST API by curl — projects, memberships, roles, milestones (sprints), epics, user stories, tasks, issues (with statuses, types, priorities, severities, points, custom attributes), wiki, history, attachments, comments, webhooks, notify policies, search, resolver, stats, and import/export. Authenticates with TAIGA_ADMIN_USERNAME + TAIGA_ADMIN_PASSWORD to obtain TAIGA_AUTH_TOKEN against TAIGA_API_URL.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/taiga-ops

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **api-reference** — This skill should be used when the user asks for "Taiga API endpoints", "Taiga REST API", "Taiga curl examples", "Taiga API documentation", the exact path/method for any Taiga resource, or needs HTTP details for projects, epics, user stories, tasks, issues, milestones, wiki, webhooks, custom attributes, search, or import/export. Index into the full per-domain endpoint catalog.
- **common-operations** — This skill should be used when the user wants to do project-management work in Taiga — "create a user story / task / issue / epic in Taiga", "start a new sprint", "move a story to In Progress", "assign a task", "comment on an issue", "add members to a Taiga project", "build a sprint report", or any everyday Taiga operation. Provides plain-language workflows that drive the REST API and route to the exact endpoints.
- **examples** — This skill should be used when the user wants a worked end-to-end Taiga example or walkthrough — "show me a full Taiga workflow", "example of setting up a project in Taiga", "how do I run sprint planning via the API", "end-to-end bug triage example", or wants to see several Taiga API calls chained together for a real scenario.
- **setup** — This skill should be used when the user wants to "connect to Taiga", "log into Taiga", "authenticate with Taiga", "get a Taiga auth token", "set up Taiga access", or before running any Taiga REST API call. Establishes the auth token and the global request conventions (headers, pagination, version locking, the resolver).
- **troubleshoot** — This skill should be used when a Taiga REST API call fails or behaves unexpectedly — "Taiga returns 401 / 403", "version conflict", "Taiga login fails", "can't find the object", "pagination missing results", "PATCH rejected", or any Taiga error response. Maps symptoms to causes and fixes.

## Subagents

Defined under `.codex/agents/` as TOML files:

- **taiga-assistant** — Use this agent when the user needs help running project-management operations in Taiga — managing projects, sprints, user stories, tasks, issues, epics, the wiki, members, custom attributes, webhooks, search, or reports — by driving the Taiga REST API.

<example>
Context: User wants to plan a sprint
user: "Start a two-week sprint in our Apollo project and pull in the top 5 backlog stories"
assistant: "I'll use the taiga-assistant agent to create the sprint and move the stories into it."
<commentary>
Sprint planning across milestones and user stories — the agent creates the milestone and bulk-assigns stories.
</commentary>
</example>

<example>
Context: User wants to triage a bug
user: "File a high-severity bug in Taiga about checkout failing, assign it to me, and attach this screenshot"
assistant: "I'll use the taiga-assistant agent to create and document the issue."
<commentary>
Issue creation with classifiers, assignment, comment, and attachment — a core Taiga ops flow.
</commentary>
</example>

<example>
Context: User wants a status report
user: "Give me a summary of open issues and remaining sprint points for project Apollo"
assistant: "I'll use the taiga-assistant agent to gather the stats and format the report."
<commentary>
Reporting from search, filtered lists, and stats endpoints — the agent aggregates and presents.
</commentary>
</example>

