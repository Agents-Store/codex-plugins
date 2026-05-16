# project-template-creator

> Manage project template hierarchy with unified improvement workflow. Route fixes to plugins or parent templates automatically, quick-capture ideas for later, and run unified end-of-session reviews covering both plugins and templates.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/project-template-creator

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **audit-stack** — Use when the user asks to "audit project stack", "analyze technologies", "scan dependencies", "generate stack.json", "what template level is this project", "map project to layers", or wants to discover all technologies in a codebase and get template and stack.json recommendations.

- **capture** — Use this skill when the user says "capture this", "note this for later", "remember to fix this", "save this improvement", "add to backlog", "I'll fix this later", or wants to quickly jot down an improvement idea without interrupting their current work. Defers routing and application to the wrap-up session.

- **create** — Use this skill when the user asks to "create a new template", "scaffold a Level 1 template", "create project-{stack}", "make a new stack template", "fork project-template", "create a demo template", "set up a new project from template", or wants to create any new project template at Level 1, 1.5, or 2 from the universal base or a stack template.

- **examples** — Use this skill when the user asks for "examples", "how does template feedback work", "show me a walkthrough", "demo the template workflow", or needs to see end-to-end scenario walkthroughs for the project-template-creator plugin.

- **feedback** — Use this skill when the user says "this should be in the parent template", "fix the template", "add this to project-template", "send feedback to parent", "improve the base template", "this skill belongs in the template", "update the parent", "push this up to the template", "the template needs this", "this is missing from the template", or discovers any issue while working in a child project that should be fixed in a parent template (Level 0 or Level 1).

- **improve** — Use this skill when the user says "improve", "this should be better", "fix this in the source", "this belongs in the plugin", "this belongs in the template", "push this upstream", "improve the plugin", "improve the template", or discovers any improvement while working in a child project that should go to either a plugin or a parent template. This is the unified entry point that auto-routes to the correct system.

- **sync** — Use this skill when the user says "sync from parent", "pull template changes", "merge parent template", "update from project-template", "my project is out of sync", "get latest template changes", "sync template", or needs to propagate improvements from a parent template (Level 0 or Level 1) down to a child project.

- **template-reference** — Use this skill when the user asks about "template hierarchy", "template levels", "project template conventions", "what files go in a template", "Level 0 vs Level 1", "template structure", "what belongs in the parent template", or needs reference documentation for the project template system and its 4-level hierarchy.

- **validate** — Use this skill when the user asks to "validate template", "check template structure", "is my template correct", "verify template conventions", "validate project template", "check template files", or needs to verify that a project template follows the Level 0/1/1.5/2 conventions and has all required files.

- **wrap-up** — Use this skill when the user says "wrap up", "end session", "done for today", "session review", "what should go into the template", "template improvements", "save template learnings", "review what we did for the template", "plugin improvements", "what should go into the plugin", or at the end of a work session to review what discoveries should be pushed up to parent templates or plugins.


## Subagents

Defined under `.codex/agents/` as TOML files:

- **template-architect** — Use this agent when the user needs help deciding where an improvement belongs
in the template hierarchy — Level 0 (universal) vs Level 1 (stack-specific) vs
project-only, or when planning a new template's structure.

<example>
Context: User is unsure where a skill should live
user: "Should this 'new-page' skill go in project-template (Level 0) or project-directus-nextjs (Level 1)?"
assistant: "I'll use the template-architect agent to analyze whether this skill is universal or stack-specific."
<commentary>
The 'new-page' skill depends on Next.js and Directus specifics, so it belongs at Level 1 (stack-specific), not Level 0 (universal).
</commentary>
</example>

<example>
Context: User found a gotcha while working and wants to push it to a template
user: "The safety rule about never force-pushing to main should probably be in the base template, but the Directus SDK caching gotcha shouldn't, right?"
assistant: "I'll use the template-architect agent to analyze the feedback routing."
<commentary>
Generic git safety → Level 0 (all stacks). Directus SDK caching → Level 1 (project-directus-nextjs only).
</commentary>
</example>

<example>
Context: User discovered a tool API pattern that a plugin should document
user: "The Directus SDK needs cache: 'no-store' on every fetch — should this go in the template or the plugin?"
assistant: "I'll use the template-architect agent to determine if this is a plugin or template improvement."
<commentary>
Tool-specific SDK knowledge → Plugin (directus-dev). Not a template concern — it's about how the tool works, not project structure.
</commentary>
</example>

<example>
Context: User wants to create a new stack template
user: "I want to create a template for Supabase + Nuxt projects"
assistant: "I'll use the template-architect agent to plan the template structure and identify which plugins to include."
<commentary>
Multi-technology template implies Level 1. The agent plans layers, identifies existing plugins, and recommends structure.
</commentary>
</example>


## Workflows (canonical slash commands)

Codex CLI doesn't support custom slash commands — invoke these workflows via natural language. Each entry below is a prompt template you can adapt:

### `audit-stack`

Scan a project's source code and produce a stack audit with technology layers, architecture recommendations, and template/stack.json generation suggestions

Arguments: `Path to project directory (defaults to current directory)`

<details><summary>Prompt template</summary>

# Audit Project Stack

Scan a project's source code, classify technologies into Logic/Interface/Data layers, and generate template and stack.json recommendations.

## Instructions

1. Read the audit-stack skill at `${CLAUDE_PLUGIN_ROOT}/skills/audit-stack/SKILL.md` — it contains the full 5-phase workflow
2. Read both reference files before starting the scan:
   - `${CLAUDE_PLUGIN_ROOT}/skills/audit-stack/references/technology-signatures.md`
   - `${CLAUDE_PLUGIN_ROOT}/skills/audit-stack/references/layer-classification.md`
3. Determine the target project directory: use `$ARGUMENTS` if provided, otherwise use the current working directory
4. Execute all 5 phases in order:
   - **Phase 1:** Source Code Scan — detect all technologies from package manifests, config files, docker-compose, imports, directory structure
   - **Phase 2:** Layer Classification — classify each technology into Logic / Interface / Data
   - **Phase 3:** Technology Research — use the best available search tools (Firecrawl > Exa > Perplexity > Jina > Context7 > WebSearch) to research each technology's ecosystem
   - **Phase 4:** Architecture Recommendation — consolidation opportunities, layer gaps, modern alternatives, integration patterns
   - **Phase 5:** Template Recommendation — determine level, generate draft stack.json, identify required plugins, output creation command
5. Present the complete 3-part report: Stack Audit, Architecture Recommendation, Template Recommendation

## Target project

$ARGUMENTS

</details>

### `capture`

Quick-capture an improvement idea without interrupting work — saves to backlog for later processing during wrap-up

Arguments: `Quick note (e.g., 'directus-dev missing cache gotcha', 'add VERCEL_TOKEN to L1 .env.example')`

<details><summary>Prompt template</summary>

Read the capture skill at `${CLAUDE_PLUGIN_ROOT}/skills/capture/SKILL.md` and follow its instructions.

Pass the user's argument as the improvement note. If no argument was provided, ask what to capture.

</details>

### `create`

Create a new project template at Level 1, 1.5, or 2 by cloning and customizing a parent template

Arguments: `Template description (e.g., 'Level 1 template for Directus + Next.js', 'demo for Supabase + Nuxt')`

<details><summary>Prompt template</summary>

# Create Project Template

Create a new project template by cloning and customizing a parent template.

## Instructions

1. Read the create skill at `${CLAUDE_PLUGIN_ROOT}/skills/create/SKILL.md`
2. Follow all steps: gather info → generate name → clone parent → customize stack.json → customize CLAUDE.md → customize files → find plugins → validate → commit
3. Use references from `${CLAUDE_PLUGIN_ROOT}/skills/create/references/` for stack.json schema and level checklists
4. For design decisions, use the `template-architect` agent
5. Run validation at the end using `${CLAUDE_PLUGIN_ROOT}/skills/validate/SKILL.md`

## User request

$ARGUMENTS

</details>

### `feedback`

Report an issue in the current project that should be fixed in a parent template (Level 0 or Level 1)

Arguments: `Description of what to fix (e.g., 'add VERCEL_TOKEN to .env.example', 'create a new-api-route skill')`

<details><summary>Prompt template</summary>

# Template Feedback

Report and fix an issue in a parent project template.

## Instructions

1. Read the feedback skill at `${CLAUDE_PLUGIN_ROOT}/skills/feedback/SKILL.md`
2. Follow all steps: identify improvement → read stack.json → locate parent → apply fix → record in LEARNINGS.md → commit
3. If the parent template cannot be found locally, offer to clone from GitHub (stackmakers-ai org)
4. For ambiguous Level 0 vs Level 1 routing, use the `template-architect` agent

## User request

$ARGUMENTS

</details>

### `improve`

Unified improvement — auto-routes fixes to the correct plugin or parent template based on what kind of knowledge it is

Arguments: `Description of what to improve (e.g., 'directus-dev sdk-patterns needs cache gotcha', 'add VERCEL_TOKEN to .env.example')`

<details><summary>Prompt template</summary>

Read the improve skill at `${CLAUDE_PLUGIN_ROOT}/skills/improve/SKILL.md` and follow its instructions.

Pass the user's argument as the improvement description. If no argument was provided, ask what needs improving.

</details>

### `sync`

Sync the current child project from its parent template — pull latest skills, commands, rules, and configs

<details><summary>Prompt template</summary>

# Sync from Parent Template

Pull improvements from a parent template into the current project.

## Instructions

1. Read the sync skill at `${CLAUDE_PLUGIN_ROOT}/skills/sync/SKILL.md`
2. Follow all steps: read stack.json → locate parent → compare files → present diff → user decision → apply → summary
3. Be careful NOT to overwrite project-specific files (stack.json, CLAUDE.md, .env.example, project-config)

## User request

$ARGUMENTS

</details>

### `validate`

Validate a project template against Level 0/1/1.5/2 conventions and required file structure

Arguments: `Path to template directory (defaults to current directory)`

<details><summary>Prompt template</summary>

# Validate Project Template

Run comprehensive compliance checks against a project template directory.

## Instructions

1. Read the validate skill at `${CLAUDE_PLUGIN_ROOT}/skills/validate/SKILL.md`
2. Determine the template directory: use `$ARGUMENTS` if provided, otherwise use the current working directory
3. Execute all validation steps: determine level, check stack.json, verify required files, check CLAUDE.md quality, verify consistency, run security scan
4. Present the final validation report
5. If issues are found, provide specific fix instructions

## Template path

$ARGUMENTS

</details>

### `wrap-up`

End-of-session review — find plugin and template improvements, process captured backlog, push fixes to sources

<details><summary>Prompt template</summary>

# Unified Session Wrap-Up

Review the current session for discoveries that should be pushed to parent templates or plugins. Also process any items captured via `/capture`.

## Instructions

1. Read the wrap-up skill at `${CLAUDE_PLUGIN_ROOT}/skills/wrap-up/SKILL.md`
2. Follow all phases: backlog → review → categorize → user decision → apply or record → summary
3. For template fixes, delegate to `${CLAUDE_PLUGIN_ROOT}/skills/feedback/SKILL.md`
4. For plugin fixes, invoke `/plugin-creator:feedback` via the Skill tool

## User request

$ARGUMENTS

</details>
