# project-template-creator

> Manage project template hierarchy with unified improvement workflow. Route fixes to plugins or parent templates automatically, quick-capture ideas for later, and run unified end-of-session reviews covering both plugins and templates.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/project-template-creator

## Skills

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

- **audit-stack** — Scan a project's source code and produce a stack audit with technology layers, architecture recommendations, and template/stack.json generation suggestions
- **capture** — Quick-capture an improvement idea without interrupting work — saves to backlog for later processing during wrap-up
- **create** — Create a new project template at Level 1, 1.5, or 2 by cloning and customizing a parent template
- **feedback** — Report an issue in the current project that should be fixed in a parent template (Level 0 or Level 1)
- **improve** — Unified improvement — auto-routes fixes to the correct plugin or parent template based on what kind of knowledge it is
- **sync** — Sync the current child project from its parent template — pull latest skills, commands, rules, and configs
- **validate** — Validate a project template against Level 0/1/1.5/2 conventions and required file structure
- **wrap-up** — End-of-session review — find plugin and template improvements, process captured backlog, push fixes to sources

## Subagents

Codex does not install plugin subagents automatically — copy them manually before use:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

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

