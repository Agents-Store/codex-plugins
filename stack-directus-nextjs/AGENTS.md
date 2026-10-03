# stack-directus-nextjs

> Directus + Next.js architecture plugin. How Directus (content, files, access) and a Next.js App Router frontend fit together: who holds the token, how the cache is invalidated, how assets and types cross the boundary, who owns the session, and a production checklist. Tool knowledge comes from its dependencies.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/stack-directus-nextjs

## Skills

- **authentication** — This skill should be used when the user wants to "add authentication to Directus + Next.js", "implement Directus login in Next.js", "use NextAuth with Directus", "use Better Auth with Directus", "protect Next.js pages with Directus auth", "choose an auth approach for Directus and Next.js", "decide who owns the session", or needs to decide who owns the end-user session and the tokens across Directus and Next.js.
- **deployment** — This skill should be used when the user wants to "set up Docker for Directus with Next.js", "run Directus locally for a Next.js app", "configure content-change webhooks", "set up revalidation when Directus content changes", "auto-update the site on content change", "production checklist for Directus + Next.js", or needs the pipeline that connects Directus edits to Next.js pages and the checks before production. For platform-specific deployment (Vercel, Dokploy, etc.), see the respective deployment plugin.
- **directus-to-nextjs** — This skill should be used when the user wants to "fetch Directus data in Next.js", "display Directus content in Next.js pages", "render Directus images in Next.js", "use Directus SDK with Server Components", "create Next.js pages from Directus collections", "add TypeScript types for Directus", "cache Directus data in Next.js", "revalidate after Directus content changes", "fix 403 on Directus images", or needs the rules where Directus data meets Next.js rendering: who holds the token, what is cached, how files are served, and how types follow the schema.
- **examples** — End-to-end scenario walkthroughs for the Directus + Next.js stack. This skill should be used when the user asks for "Directus Next.js examples", "how to build a blog with Directus and Next.js", "Directus Next.js product catalog", "show me a complete example", or needs implementation references for common application types.
- **full-feature** — This skill should be used when the user wants to "build a complete feature with Directus and Next.js", "create an end-to-end page from Directus data", "implement a full CRUD feature across Directus and Next.js", "build a new section of the site", "create a page that shows Directus data", "build a dashboard with Directus content", "display Directus collection in Next.js", "add a new page with CMS data", or needs a step-by-step recipe for building features that span both Directus and Next.js.
- **init-project** — This skill should be used when the user asks to "set up Directus + Next.js project", "initialize Directus Next.js stack", "configure Directus with Next.js", "connect Directus to Next.js", "bootstrap Directus Next.js app", "create Next.js app with Directus", "scaffold Directus + Next.js", "start new project with Directus", "create a new Directus Next.js project", or needs to set up environment variables and verify connections for the Directus + Next.js stack.

## Subagents

Codex does not install plugin subagents automatically — copy them manually before use:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- **stack-orchestrator** — Use this agent when the user needs help coordinating work across Directus and Next.js — building pages that display Directus content, deciding who owns the user session, configuring cache revalidation, or implementing features that span both services.

<example>
Context: User wants to build a page that displays Directus content
user: "Build a blog listing page that fetches posts from Directus and displays them with images"
assistant: "I'll use the stack-orchestrator agent to build the blog page with Directus data fetching and image handling."
<commentary>
Feature requires Directus SDK data fetching in a Next.js Server Component with image optimization — spans Data and Interface layers.
</commentary>
</example>

<example>
Context: User wants to add authentication
user: "Set up user login for the site that uses Directus"
assistant: "I'll use the stack-orchestrator agent to decide who owns the session and wire the matching authentication path."
<commentary>
Authentication spans Directus (user store, token management) and Next.js (session library, proxy) — orchestrator decides who owns the session and coordinates both sides.
</commentary>
</example>

<example>
Context: User wants to set up content-change revalidation
user: "Set up auto-rebuild when Directus content changes"
assistant: "I'll use the stack-orchestrator agent to configure the Directus Flow + tag revalidation pipeline."
<commentary>
Revalidation wiring spans Directus Flows and Next.js route handlers and cache tags — cross-service coordination.
</commentary>
</example>

<example>
Context: User encounters a cross-service issue
user: "My Directus images aren't loading in production"
assistant: "I'll use the stack-orchestrator agent to diagnose the image loading issue across Directus CORS, Next.js image config, and hosting settings."
<commentary>
Cross-service debugging requires understanding Directus file permissions, next.config.ts image patterns, and environment variables.
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

```
