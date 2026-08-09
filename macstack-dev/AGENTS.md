# macstack-dev

> MACSTACK dev plugin for Agents Store. Creates and maintains macstack.json — the standardized business + technical stack specification for Claude projects: init in existing projects, generate from scratch (result-first), discover context plugins and prototypes, scaffold project files in the prototype → stack plugins → dev plugins order, wire Infisical env, install best-practice rules and commands.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/macstack-dev

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **best-practices** — This skill should be used when the user asks to "install best practice rules", "set up project rules", "add project rules and commands", "set up project conventions", or scaffold-project reaches the rules step. Installs the proven MACSTACK rule set (.claude/rules) and core commands into a project.
- **discover-context** — This skill should be used when the user asks to "find plugins for this stack", "discover context for the project", "which plugins should I install", "find a prototype", "pick a stack prototype", or when init-project/generate-stack need context.plugins and prototype candidates. Searches Agents Store plugins and stackmakers-ai prototypes on GitHub.
- **examples** — This skill should be used when the user asks for "macstack examples", "show a full macstack.json example", "how does a complete macstack.json look", "walk me through a macstack scenario", or needs an end-to-end scenario walkthrough for this plugin's skills.
- **feedback** — This skill should be used when the user reports a problem with macstack-dev or the MACSTACK standard — "this skill did the wrong thing", "the schema is missing a field", "the passport for X is wrong", "send macstack feedback", "improve the schema based on my edit", "fix the macstack plugin" — and the fix must land in the right source (plugin, schema repo, or registry repo).
- **generate-stack** — This skill should be used when the user asks to "generate macstack.json from scratch", "design a stack for…", "pick software and architecture for my need", "create a stack spec from my request", or describes a business need without an existing codebase. Designs goals, results, processes, workflows, software and architecture result-first and produces a validated macstack.json.
- **infisical-env** — This skill should be used when the user asks to "set up Infisical for this project", "create .infisical.json", "pull the env keys", "wire the env", "sync secrets", or scaffold-project reaches the env step. Creates .infisical.json, pulls .env.prod/.env.dev, ensures every key from macstack.json resources.accesses exists, and installs the mandatory secrets scripts and commands.
- **init-project** — This skill should be used when the user asks to "create macstack.json in this project", "add macstack.json", "init macstack", "describe this existing project as macstack.json", or an existing codebase has no macstack.json. Audits the existing project and produces a validated macstack.json draft.
- **lint** — This skill should be used when the user asks to "validate macstack.json", "lint macstack", "check the stack spec", "verify macstack.json integrity", or after any skill of this plugin writes/edits macstack.json. Validates against the bundled JSON Schema and the referential-integrity rules.
- **scaffold-project** — This skill should be used when the user asks to "scaffold the project from macstack.json", "create the project working files", "generate project files from the spec", "build the project from macstack.json", or after a macstack.json is validated and the working tree must be built. Creates project files strictly in the prototype → stack plugins → dev plugins order.
- **setup** — This skill should be used when the user asks "what is macstack.json", "set up macstack", "check macstack setup", "verify macstack.json", "explain the macstack standard", or before any other macstack-dev skill runs in a project for the first time. Explains the standard, locates the schema and category registry, and verifies tooling.
- **troubleshoot** — This skill should be used when the user reports "macstack lint fails", "prototype does not resolve", "env keys missing", "scaffold broke my files", "cross-stack reference does not work", or any macstack-dev skill errors out. Diagnoses the common failure modes of the macstack.json toolchain.

## Subagents

Defined under `.codex/agents/` as TOML files:

- **macstack-architect** — Use this agent when the user needs to design or evolve a macstack.json — generate a stack from a business request, audit an existing project into a spec, choose software/architecture/prototype, or decompose goals into results, processes and workflows.

<example>
Context: User describes a business need without a codebase
user: "Design a stack for an online school: payments, an LMS, email campaigns"
assistant: "I'll use the macstack-architect agent to design the stack result-first."
<commentary>
Business request → goals/results → processes → software selection → macstack.json draft.
</commentary>
</example>

<example>
Context: Existing repo without a spec
user: "Describe this project as a macstack.json"
assistant: "I'll use the macstack-architect agent to audit the codebase and draft the spec."
<commentary>
Audit manifests/compose/.mcp.json → software/entities/workflows; ask the user only the business gaps.
</commentary>
</example>

<example>
Context: Ambiguous software choice
user: "What should I pick for a CRM stack — NocoBase or Directus?"
assistant: "I'll use the macstack-architect agent to compare against the requirements and recommend."
<commentary>
The decision needs the result-first framework and the Agentic IT Ready criteria.
</commentary>
</example>


## Workflows (canonical slash commands)

Codex CLI doesn't support custom slash commands — invoke these workflows via natural language. Each entry below is a prompt template you can adapt:

### `feedback`

Report a problem with macstack-dev, the macstack.json schema, or the registry — and fix it at the source

Arguments: `<what went wrong>`

<details><summary>Prompt template</summary>

Use the macstack-dev:feedback skill for: $ARGUMENTS. Route the fix to the right
source of truth (plugin skills / github.com/macstacks/macstack schema+examples /
github.com/macstacks/registry), apply it, sync every mirror (the plugin's bundled
schema copy, project mirrors), record the learning in LEARNINGS.md, bump versions
where required, and report the commits/PRs.

</details>

### `generate`

Generate macstack.json from scratch — result-first stack design from a business request

Arguments: `<business request>`

<details><summary>Prompt template</summary>

Use the macstack-dev:generate-stack skill (delegate design to the macstack-architect
agent for complex requests) to design a stack for: $ARGUMENTS. Then run
macstack-dev:discover-context to fill prototype and context.plugins, and
macstack-dev:lint. Present goals/results first and wait for the user to confirm the
RESULTS before offering /macstack-dev:scaffold.

</details>

### `init`

Create macstack.json in an existing project (audit codebase → validated spec)

Arguments: `[path]`

<details><summary>Prompt template</summary>

Use the macstack-dev:setup skill to verify tooling, then the macstack-dev:init-project
skill to audit the existing project ($ARGUMENTS or current directory) and produce a
validated macstack.json. Finish with macstack-dev:lint and add the CLAUDE.md
"Stack Specification" section. Report: what was derived from code, what came from the
user, and the remaining open questions.

</details>

### `lint`

Validate macstack.json against the JSON Schema and referential-integrity rules

<details><summary>Prompt template</summary>

Use the macstack-dev:lint skill on the project's macstack.json (resolve prototype
chain first if set). Output ERRORS, then WARNINGS, then the OK line. If errors exist,
propose concrete fixes and apply them on user confirmation.

</details>

### `scaffold`

Scaffold project files from macstack.json (prototype → stack plugins → dev plugins)

<details><summary>Prompt template</summary>

Use the macstack-dev:scaffold-project skill on the current project's macstack.json.
Follow the mandatory source order: 1) prototype (github or local absolute path),
2) stack plugins (architecture), 3) dev plugins (how to build with each software) —
then generate the files. Always invoke macstack-dev:infisical-env (.infisical.json +
.env.prod/.env.dev + secrets scripts) and macstack-dev:best-practices (rules +
commands). Finish with macstack-dev:lint and report created files by source.

</details>

### `sync`

Update macstack.json and derived files after stack changes (spec = definition of done)

<details><summary>Prompt template</summary>

Compare the current project state against macstack.json: new/removed software,
workflows, triggers, entities, interfaces, env keys. Update macstack.json to match
reality, regenerate derived artifacts (.env.example from resources.accesses,
CLAUDE.md Tech Stack section, enabledPlugins), run macstack-dev:lint, and remind the
user to commit the spec together with the code change (macstack-sync rule).

</details>
