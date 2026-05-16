# stack-directus-nextjs-trigger-dev

> Directus + Next.js + Trigger.dev stack dev plugin. Adds self-hosted Trigger.dev as a workflow engine for AI agents, durable async logic, and scheduled jobs on top of the Directus + Next.js App Router stack.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/stack-directus-nextjs-trigger-dev

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
args = ["trigger.dev@latest","mcp"]

[mcp_servers.trigger-dev.env]
"TRIGGER_ACCESS_TOKEN" = "${TRIGGER_SECRET_KEY}"
"TRIGGER_API_URL" = "${TRIGGER_API_URL}"

```

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **authentication** — This skill should be used when the user wants to "add authentication to Directus + Next.js", "implement Directus login in Next.js", "use NextAuth with Directus", "protect Next.js pages with Directus auth", "set up cookie-based auth for Directus SSR", or needs patterns for authenticating users across Directus and Next.js.
- **background-tasks** — This skill should be used when the user wants to "offload work to trigger.dev from next.js", "run background job from a server action", "trigger a task from a route handler", "call tasks.trigger from next.js app router", "delegate slow work to trigger.dev", "show realtime task status in next.js", "fix force-dynamic error with trigger.dev", "handle onclick trigger.dev server action", or needs the integration patterns between Next.js (App Router) and Trigger.dev for event-driven background work.
- **deployment** — This skill should be used when the user wants to "set up Docker for Directus locally", "deploy trigger.dev tasks", "run local dev for the 3-service stack", "configure content-change webhooks with trigger.dev", "CI/CD for trigger tasks", "production checklist for directus + nextjs + trigger.dev", or needs local dev and integration deployment patterns. For platform-specific hosting (Vercel, Dokploy, etc.), see the respective deployment plugin.
- **directus-to-nextjs** — This skill should be used when the user wants to "fetch Directus data in Next.js", "display Directus content in Next.js pages", "render Directus images in Next.js", "use Directus SDK with Server Components", "create Next.js pages from Directus collections", "add TypeScript types for Directus", or needs integration patterns between Directus data and Next.js rendering.
- **directus-to-trigger** — This skill should be used when the user wants to "trigger a task from a directus flow", "run a background task when a directus item is created or updated", "forward directus webhooks to trigger.dev", "process directus items asynchronously", "build a directus automate → next.js → trigger pipeline", "have a task write back to directus after processing", or needs the pattern for wiring Directus Flow events through Next.js into Trigger.dev tasks and back.
- **examples** — End-to-end scenario walkthroughs for the Directus + Next.js + Trigger.dev stack. This skill should be used when the user asks for "directus nextjs trigger.dev examples", "how to build a blog with directus + next.js", "ai enrichment pipeline example", "scheduled data sync example", "show me a complete example with background tasks", or needs implementation references for common application types on this stack.
- **full-feature** — This skill should be used when the user wants to "build a complete feature with directus nextjs and trigger.dev", "create an end-to-end feature with background tasks", "implement a full crud feature with async processing", "build a new section of the site that uses background jobs", "add a page backed by directus with a background task", or needs a step-by-step recipe for building features that span Directus, Next.js, AND Trigger.dev.
- **init-project** — This skill should be used when the user asks to "set up Directus + Next.js + Trigger.dev project", "initialize directus nextjs trigger.dev stack", "bootstrap the 3-service stack", "configure directus next.js and trigger.dev together", "connect trigger.dev to a directus nextjs app", "scaffold stack with background tasks", "start a new project with background jobs", or needs to set up environment variables and verify connections for the Directus + Next.js + Trigger.dev stack.
- **scheduled-tasks** — This skill should be used when the user wants to "create a scheduled task with trigger.dev", "add a cron job", "run a daily task", "sync directus on a schedule", "build a cron that reads from directus", "attach a schedule to production", "timezone-aware cron", or needs patterns for defining `schedules.task()` cron jobs that interact with Directus from the Directus + Next.js + Trigger.dev stack.

## Subagents

Defined under `.codex/agents/` as TOML files:

- **stack-orchestrator** — Use this agent when the user needs help coordinating work across Directus, Next.js, and Trigger.dev — building pages that display Directus content, setting up authentication, offloading slow work to background tasks, defining scheduled jobs, or implementing features that span all three services.

<example>
Context: User wants to build a page that displays Directus content
user: "Build a blog listing page that fetches posts from Directus and displays them with images"
assistant: "I'll use the stack-orchestrator agent to build the blog page with Directus data fetching and image handling."
<commentary>
Feature requires Directus SDK data fetching in a Next.js Server Component with image optimization — spans Data and Interface layers.
</commentary>
</example>

<example>
Context: User wants to add a background enrichment pipeline
user: "When an article is created in Directus, automatically enrich it with AI-generated tags and a summary"
assistant: "I'll use the stack-orchestrator agent to wire the Directus Flow → webhook → Trigger task → Directus writeback pipeline."
<commentary>
Feature spans all three services: Directus Flow event, Next.js webhook receiver, Trigger.dev task body, Directus SDK writeback, ISR revalidation. Exactly what the orchestrator exists for.
</commentary>
</example>

<example>
Context: User wants a scheduled data sync
user: "Pull currency rates from an external API every day at 2am and store them in Directus"
assistant: "I'll use the stack-orchestrator agent to define a scheduled Trigger.dev task that writes to Directus and revalidates the Next.js dashboard."
<commentary>
Requires `schedules.task()`, Directus SDK inside a task, and cache revalidation — routed through the scheduled-tasks skill.
</commentary>
</example>

<example>
Context: User hits a CI build failure with Trigger.dev
user: "My CI build is failing with 'TRIGGER_SECRET_KEY is required' on a route that calls tasks.trigger()"
assistant: "I'll use the stack-orchestrator agent to diagnose and fix the static-generation issue."
<commentary>
Classic `force-dynamic` gotcha — orchestrator routes to background-tasks skill which documents the fix.
</commentary>
</example>

<example>
Context: User wants to add authentication
user: "Set up user login using Directus authentication and NextAuth"
assistant: "I'll use the stack-orchestrator agent to implement the Directus + NextAuth authentication flow."
<commentary>
Authentication spans Directus (user store, token management) and Next.js (NextAuth, middleware, session) — orchestrator coordinates both sides.
</commentary>
</example>

<example>
Context: User wants to deploy the full stack
user: "Deploy my Next.js app and set up auto-rebuild when Directus content changes, and deploy my Trigger.dev tasks"
assistant: "I'll use the stack-orchestrator agent to configure deployment across all three services."
<commentary>
Deployment involves Next.js host env vars, Directus Automate flows, webhook connections, AND Trigger.dev task deploy via `npx trigger.dev@latest deploy --self-hosted`. Cross-service coordination is the orchestrator's job.
</commentary>
</example>

<example>
Context: User encounters a cross-service issue
user: "My Directus images aren't loading in production"
assistant: "I'll use the stack-orchestrator agent to diagnose the image loading issue across Directus CORS, Next.js image config, and hosting settings."
<commentary>
Cross-service debugging requires understanding Directus CORS, next.config.ts remotePatterns, and environment variables.
</commentary>
</example>

