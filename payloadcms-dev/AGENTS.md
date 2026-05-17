# payloadcms-dev

> PayloadCMS dev plugin for Agents Store. Covers collections, fields, hooks, access control, queries, adapters, Lexical rich text, jobs queue, plugin development, Next.js integration, CLI, migrations, and end-to-end scenarios for TypeScript developers building with Payload v3.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/payloadcms-dev

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **access-control** — This skill should be used when the user asks about "Payload access control", "RBAC in Payload", "row-level security", "isAdmin function", "field access control", "global access control", "overrideAccess", "multi-tenant Payload", "filter by user role", "Payload Access function", or needs to decide who can create/read/update/delete documents.
- **adapters** — This skill should be used when the user asks about "Payload database adapter", "Postgres in Payload", "MongoDB Payload setup", "SQLite Payload", "S3 storage adapter", "Cloudflare R2 Payload", "Vercel Blob upload", "Payload email Resend", "Payload Nodemailer SMTP", "Payload transactions", or needs to wire Payload to a database, file storage, or email provider.
- **api-reference** — This skill should be used when the user asks for "PayloadCMS REST endpoint", "Payload curl example", "Payload GraphQL query syntax", "Payload Local API method signature", "Payload login endpoint", "Payload auth headers", or needs the exact HTTP/method signature for a Payload API call.
- **cli-recipes** — This skill should be used when the user asks about "payload migrate", "payload generate:types", "payload generate:importmap", "payload migrate:create", "payload migrate:down", "payload migrate:reset", "payload migrate:refresh", "payload migrate:status", "Payload CLI commands", or needs to run the Payload command-line tool for schema migrations or codegen.
- **cms-migration** — This skill should be used when the user asks to "migrate WordPress to Payload", "move content from Contentful to Payload", "import Strapi data into Payload", "migrate Sanity to PayloadCMS", "Webflow CMS to Payload", "design Payload collections from CMS export", or needs a structured workflow for moving content from another CMS into Payload.
- **collections** — This skill should be used when the user asks to "create a Payload collection", "define a CollectionConfig", "set up an auth collection", "build an upload collection", "add drafts/versions", "configure admin panel for a collection", "enable live preview", "set defaultColumns", or needs to model any content type in PayloadCMS v3.
- **examples** — This skill should be used when the user asks "show me a complete Payload example", "give me a working Payload blog", "Payload ecommerce example", "Payload auth-only API example", "Payload jobs worker example", "Payload multi-tenant example", or wants end-to-end scenario walkthroughs instead of isolated snippets.
- **fields** — This skill should be used when the user asks about "Payload field types", "add a relationship field", "blocks field", "array field", "rich text field", "upload field", "virtual field", "conditional fields", "field validation", "join field", "point/geolocation field", "slug field helper", or needs to design any field inside a PayloadCMS collection or global.
- **hooks** — This skill should be used when the user asks about "Payload hooks", "beforeChange", "afterChange", "afterRead", "beforeDelete", "field hooks", "global hooks", "prevent hook loops", "Next.js revalidation in Payload", "transaction safe hooks", "auto-set author from req.user", or needs to wire up lifecycle automation in PayloadCMS.
- **jobs-queue** — This skill should be used when the user asks about "Payload jobs queue", "Payload background tasks", "Payload workflows", "Payload cron scheduling", "Payload task retries", "Payload runJobs", "Payload autoRun", "queue a job in Payload", or needs to run any background or scheduled work in PayloadCMS.
- **lexical-editor** — This skill should be used when the user asks about "Payload rich text", "Lexical editor in Payload", "custom Lexical feature", "richText blocks", "richText link/upload/relationship", "custom Lexical node", "render Payload Lexical to JSX", "convert Lexical to HTML", or needs to customize the editor inside `richText` fields.
- **nextjs-integration** — This skill should be used when the user asks about "Payload with Next.js", "getPayload in server component", "Payload App Router", "Payload route groups", "Payload live preview Next.js", "revalidate Payload page", "Payload server actions", "Payload draft mode", "Payload Next.js cache", or needs to wire PayloadCMS into a Next.js v14/v15 frontend.
- **plugin-development** — This skill should be used when the user asks to "build a Payload plugin", "create payload-plugin package", "write a Payload plugin from scratch", "add fields via plugin", "preserve hooks in plugin", "publish payload-plugin to npm", "plugin architecture in Payload", or needs to author or maintain a reusable PayloadCMS plugin.
- **queries** — This skill should be used when the user asks about "Payload Local API", "payload.find", "payload.findByID", "where query", "Payload query operators", "depth and populate", "filter Payload by relationship", "sort and paginate Payload results", "Payload REST API query string", "GraphQL queries", or needs to read or write data with Payload.
- **setup** — This skill should be used when the user asks to "install PayloadCMS", "create a Payload project", "set up Payload v3", "scaffold Payload app", "initialize Payload with Next.js", "pick a Payload database adapter", "configure payload.config.ts", or needs to bootstrap a fresh Payload project from zero to a running admin panel.
- **troubleshoot** — This skill should be used when the user asks about "Payload error", "Payload not working", "Payload TypeError", "access bypass in Local API", "Payload hook infinite loop", "Payload transaction rollback", "Cannot find module payload", "Payload import map missing", "Payload type generation fails", "Could not resolve component", or sees a stack trace from Payload they want decoded.

## Subagents

Defined under `.codex/agents/` as TOML files:

- **payloadcms-developer** — Use this agent when the user is actively building, debugging, or extending a PayloadCMS v3 project — designing collections and fields, wiring hooks and access control, integrating Payload with Next.js, building third-party Payload plugins, migrating from another CMS, or troubleshooting a Payload error.

<example>
Context: The user is starting a new Payload-backed project.
user: "Help me design collections for a SaaS that has projects, project members, and timesheet entries."
assistant: "I'll use the payloadcms-developer agent to design the data model end-to-end."
<commentary>This is multi-collection schema design with access control implications — the developer agent loads the right Payload skills and proposes a typed, tenant-aware design.</commentary>
</example>

<example>
Context: A hook is misbehaving in production.
user: "My afterChange hook keeps firing in a loop and exhausting DB connections."
assistant: "Let me use the payloadcms-developer agent to diagnose the loop and apply the context-flag pattern."
<commentary>The agent invokes the troubleshoot and hooks skills, then proposes a targeted fix with the req.context guard pattern.</commentary>
</example>

<example>
Context: The user is authoring a reusable Payload plugin.
user: "I want to publish a payload-plugin-sentry package that adds error reporting to every collection's afterError hook."
assistant: "I'll use the payloadcms-developer agent — this needs the plugin-development conventions and the right (options) => (config) pattern."
<commentary>Plugin authoring has specific structural rules (SWC build, multi-export entrypoints, hook-array preservation) that the agent enforces via the plugin-development skill.</commentary>
</example>


## Workflows (canonical slash commands)

Codex CLI doesn't support custom slash commands — invoke these workflows via natural language. Each entry below is a prompt template you can adapt:

### `scaffold`

Scaffold a fresh PayloadCMS v3 project with create-payload-app, walking through database adapter, template, and package manager choices.

Arguments: `project-name`

<details><summary>Prompt template</summary>

# /payloadcms-dev:scaffold

Bootstrap a new PayloadCMS v3 project from zero. Wraps `npx create-payload-app@latest` with the recommended Next.js App Router defaults and walks the user through every decision before running the scaffolder.

## Step 1 — Collect Inputs

Use `AskUserQuestion` to gather (skip any the user already supplied as `$ARGUMENTS`):

1. **Project name** — kebab-case directory name (e.g. `my-cms`). If `$ARGUMENTS` is non-empty, use it as the default.
2. **Database adapter** — options:
   - **PostgreSQL** (recommended for prod) — needs a `DATABASE_URI` connection string.
   - **MongoDB** — needs an `mongodb://` URI, ideally a replica set for transactions.
   - **SQLite** (libSQL) — `file:./payload.db` for local, `libsql://…` + auth token for Turso.
3. **Template** — `blank` (recommended for custom builds) / `website` (marketing site demo) / `ecommerce` (Stripe demo).
4. **Package manager** — pnpm (default) / npm / yarn / bun.

## Step 2 — Verify Prerequisites

Run a Bash check before scaffolding:

```bash
node --version          # Must be >= 20.9.0
which pnpm || which npm || which yarn || which bun
```

If Node is below 20.9.0, tell the user to install LTS Node via `nvm install --lts && nvm use --lts` and stop here.

## Step 3 — Run the Scaffolder

Construct and run the `create-payload-app` command from the chosen inputs:

```bash
pnpm create payload-app@latest <project-name> \
  --template <blank|website|ecommerce> \
  --db <postgres|mongodb|sqlite> \
  --use-pnpm
```

Substitute `--use-npm` / `--use-yarn` / `--use-bun` as appropriate. Run in the user's current working directory.

## Step 4 — Post-Install Walkthrough

After the scaffolder finishes:

1. **Show generated `.env`** — read it back and explain each var.
2. **Generate `PAYLOAD_SECRET`** if the placeholder is still there:
   ```bash
   openssl rand -hex 32
   ```
   Write it into `.env` with `Edit`.
3. **Confirm `DATABASE_URI`** — prompt the user to fill in their actual connection string. If they're using Postgres and need a local DB, suggest a Docker one-liner:
   ```bash
   docker run --name payload-postgres -e POSTGRES_PASSWORD=postgres -p 5432:5432 -d postgres:16
   # → postgres://postgres:postgres@localhost:5432/postgres
   ```
4. **Install dependencies** if `--no-deps` was used:
   ```bash
   cd <project-name> && pnpm install
   ```
5. **Run the dev server**:
   ```bash
   cd <project-name> && pnpm dev
   ```
6. **Tell the user to open `http://localhost:3000/admin`** and create the first admin user.

## Step 5 — Suggest Next Steps

Don't auto-invoke other skills. Tell the user to invoke the relevant ones based on what they want to do next:

- "Design my first content collection" → invoke `collections` skill.
- "Pick the right field types" → invoke `fields` skill.
- "Decide what storage adapter to use for uploads" → invoke `adapters` skill.
- "Wire access control" → invoke `access-control` skill.
- "Browse a complete blog example" → invoke `examples` skill.

## Failure Modes

- **`create-payload-app` errors with EACCES** → suggest `sudo chown -R $(whoami) ~/.npm` and rerun.
- **`Node version too low`** → install Node 20.9+ via nvm.
- **`Cannot find module 'sharp'`** after install → run `pnpm add sharp` inside the project, then retry `pnpm dev`.
- **Database connection refused** → confirm the DB is reachable from the dev machine and the URI matches.

## Notes

- Do not modify `payload.config.ts` automatically — the scaffolder writes a known-good config. Customizations belong in follow-up skill-driven work.
- Do not enable `db.push: false` until the user has migrations in place — for dev, `push: true` is the right default (the scaffolder sets this).
- Do not run `pnpm migrate` on a fresh project — the dev server's auto-push handles dev schema sync.

## $ARGUMENTS

If $ARGUMENTS contains a project name, use it as the default for Step 1 and proceed.

</details>
