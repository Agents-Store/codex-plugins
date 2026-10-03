# stack-directus-nextjs-trigger

> Directus + Next.js + Trigger.dev architecture plugin. How Directus (content, files, access), a Next.js App Router frontend and self-hosted Trigger.dev (durable and scheduled work) fit together: who holds which token, how the cache is invalidated, how a Directus change reaches a task and the result reaches the page, who owns the session, and a production checklist. Tool knowledge comes from its dependencies.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/stack-directus-nextjs-trigger

## Skills

- **authentication** — This skill should be used when the user wants to "add authentication to Directus + Next.js", "implement Directus login in Next.js", "use NextAuth with Directus", "use Better Auth with Directus", "protect Next.js pages with Directus auth", "choose an auth approach for Directus and Next.js", "decide who owns the session", "act as the signed-in user inside a trigger.dev task", or needs to decide who owns the end-user session and the tokens across Directus and Next.js.
- **background-tasks** — This skill should be used when the user wants to "offload work to trigger.dev from next.js", "run a background job from a server action", "trigger a task from a route handler", "delegate slow work to trigger.dev", "decide what runs in a Server Action and what in a task", "show a task's progress in next.js", "give the browser a token for a trigger.dev run", "keep task environment variables apart from next.js", "fix a TRIGGER_SECRET_KEY build error", or needs the rules where a Next.js App Router app hands work to Trigger.dev and gets the result back.
- **deployment** — This skill should be used when the user wants to "run the Directus Next.js Trigger.dev stack locally", "deploy trigger.dev tasks next to a Next.js app", "set environment variables for trigger.dev tasks", "configure content-change webhooks with trigger.dev", "revalidate the site when a task changes Directus content", "CI/CD for trigger tasks", "production checklist for directus + nextjs + trigger.dev", or needs the pipelines that connect the three services and the checks before production. For platform-specific hosting (Vercel, Dokploy, etc.), see the respective deployment plugin.
- **directus-to-nextjs** — This skill should be used when the user wants to "fetch Directus data in Next.js", "display Directus content in Next.js pages", "render Directus images in Next.js", "use Directus SDK with Server Components", "create Next.js pages from Directus collections", "add TypeScript types for Directus", "cache Directus data in Next.js", "revalidate after Directus content changes", "fix 403 on Directus images", or needs the rules where Directus data meets Next.js rendering: who holds the token, what is cached, how files are served, and how types follow the schema.
- **directus-to-trigger** — This skill should be used when the user wants to "trigger a task from a directus flow", "run a background task when a directus item is created or updated", "forward directus webhooks to trigger.dev", "process directus items asynchronously", "build a directus flow to next.js to trigger pipeline", "have a task write back to directus", "stop a directus flow and task from looping", "read and write directus from a trigger.dev task", or needs the pattern for wiring Directus Flow events through Next.js into Trigger.dev tasks and back.
- **examples** — End-to-end scenario walkthroughs for the Directus + Next.js + Trigger.dev stack. This skill should be used when the user asks for "directus nextjs trigger.dev examples", "how to build a blog with directus + next.js", "ai enrichment pipeline example", "scheduled data sync example", "show me a complete example with background tasks", or needs implementation references for common application types on this stack.
- **full-feature** — This skill should be used when the user wants to "build a complete feature with directus nextjs and trigger.dev", "create an end-to-end feature with background tasks", "implement a full crud feature with async processing", "build a new section of the site that uses background jobs", "add a page backed by directus with a background task", or needs a step-by-step recipe for building features that span Directus, Next.js, and Trigger.dev.
- **init-project** — This skill should be used when the user asks to "set up Directus + Next.js + Trigger.dev project", "initialize directus nextjs trigger.dev stack", "bootstrap the 3-service stack", "configure directus next.js and trigger.dev together", "connect trigger.dev to a directus nextjs app", "scaffold stack with background tasks", "start a new project with background jobs", or needs to set up environment variables, versions and verify connections for the Directus + Next.js + Trigger.dev stack.

## Subagents

Codex does not install plugin subagents automatically — copy them manually before use:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- **stack-orchestrator** — Use this agent when the user needs help coordinating work across Directus, Next.js, and Trigger.dev: building pages that display Directus content, deciding who owns the user session, offloading slow work to background tasks, wiring a Directus change to a task, defining scheduled jobs, or implementing features that span all three services.

<example>
Context: User wants to build a page that displays Directus content
user: "Build a blog listing page that fetches posts from Directus and displays them with images"
assistant: "I'll use the stack-orchestrator agent to build the blog page with tagged Directus reads and image handling."
<commentary>
The feature needs Directus SDK reads in a Next.js Server Component with the cache tags and image rules of the stack: it spans the Data and Interface layers.
</commentary>
</example>

<example>
Context: User wants to add a background enrichment pipeline
user: "When an article is created in Directus, automatically enrich it with AI-generated tags and a summary"
assistant: "I'll use the stack-orchestrator agent to wire the Directus Flow, the Next.js receiver, the Trigger.dev task and the write-back."
<commentary>
The feature spans all three services: a Flow with a loop guard, a receiver that starts a debounced task, a task with its own Directus token, and the page refresh. This is what the orchestrator exists for.
</commentary>
</example>

<example>
Context: User wants a scheduled data sync
user: "Pull currency rates from an external API every day at 2am and store them in Directus"
assistant: "I'll use the stack-orchestrator agent to define a scheduled Trigger.dev task that upserts into Directus and refreshes the dashboard."
<commentary>
Needs a declarative schedule, the task-side Directus client, an idempotent upsert and the revalidation path; the schedule rules come from the trigger-dev plugin.
</commentary>
</example>

<example>
Context: User hits a build failure with Trigger.dev
user: "My CI build is failing with 'You need to set the TRIGGER_SECRET_KEY environment variable' on a route that calls tasks.trigger()"
assistant: "I'll use the stack-orchestrator agent to find where the trigger runs at build time."
<commentary>
The SDK reads the environment on the first call, not on import: a trigger at module level or during prerendering is the cause. The background-tasks skill has the rule.
</commentary>
</example>

<example>
Context: User wants to add authentication
user: "Set up user login using Directus authentication and NextAuth"
assistant: "I'll use the stack-orchestrator agent to decide who owns the session and wire the matching authentication path."
<commentary>
Authentication spans Directus (user store, token management) and Next.js (session library, proxy); the orchestrator decides who owns the session and coordinates both sides, and keeps user tokens out of tasks.
</commentary>
</example>

<example>
Context: User wants to deploy the full stack
user: "Deploy my Next.js app and set up auto-rebuild when Directus content changes, and deploy my Trigger.dev tasks"
assistant: "I'll use the stack-orchestrator agent to configure the pipelines and the environment variables across all three services."
<commentary>
Deployment involves the Next.js host variables, the Directus Flows and their secrets, the Trigger.dev environment, and a separate task deploy with a pinned CLI.
</commentary>
</example>

<example>
Context: User encounters a cross-service issue
user: "My Directus images aren't loading in production"
assistant: "I'll use the stack-orchestrator agent to diagnose the image loading issue across Directus permissions, Next.js image config, and hosting settings."
<commentary>
Cross-service debugging requires understanding Directus file permissions, the images block of next.config.ts, and environment variables.
</commentary>
</example>


## MCP servers

Add to `~/.codex/config.toml`:

```toml
[mcp_servers.directus]
url = "${NEXT_PUBLIC_DIRECTUS_URL}/mcp"
type = "http"

[mcp_servers.directus.headers]
"Authorization" = "Bearer ${DIRECTUS_ADMIN_TOKEN}"

[mcp_servers.trigger-dev]
command = "npx"
args = ["trigger.dev@${TRIGGER_CLI_VERSION:-latest}","mcp","--dev-only","--project-ref","${TRIGGER_PROJECT_REF}"]

[mcp_servers.trigger-dev.env]
"TRIGGER_ACCESS_TOKEN" = "${TRIGGER_ACCESS_TOKEN}"
"TRIGGER_API_URL" = "${TRIGGER_API_URL}"

```
