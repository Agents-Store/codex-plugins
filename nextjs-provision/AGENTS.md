# nextjs-provision

> Next.js provisioning plugin. Set up shadcn/ui and shadcn studio — component installation, theme configuration, MCP server setup, project scaffolding, and multi-registry component search across the 400+ registries of the official directory.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/nextjs-provision

## Skills

- **component-registry** — Install and use shadcn/ui and shadcn studio components with the shadcn CLI v4 — add, view, --dry-run and --diff, the @shadcn-studio and @ss-* registries, compositions that are not registry items such as form and date picker, and Base UI specifics like the render prop instead of asChild. Use when adding or composing components. Searching community registries is component-search.
- **component-search** — Search and install components from the 400+ registries in the official shadcn directory — community registries such as magicui or aceternity, registries.json with its health status, adding registries to components.json, private GitHub registries. Use when looking for a component beyond the standard shadcn/ui and shadcn studio registries.
- **examples** — End-to-end scenario walkthroughs for setting up Next.js projects with shadcn/ui and shadcn studio. This skill should be used when the user asks for "shadcn setup walkthrough", "how to set up a project with shadcn from scratch", "add shadcn to existing project example", "full shadcn setup guide", "shadcn studio tutorial", "step-by-step shadcn setup", or needs a complete example of provisioning a Next.js project with shadcn components.

- **mcp-tools** — Set up and use the official shadcn MCP server (npx shadcn@latest mcp) — mcp init per client for Claude Code, Cursor, VS Code, Codex and opencode, its seven tools, how items are addressed as @registry/item, and why the community shadcn-ui-mcp-server is no longer shipped. Use when configuring or using MCP for shadcn component work.
- **project-scaffolding** — Scaffold and organise Next.js projects that use shadcn/ui — init with templates and the base choice (Base UI or Radix), shadcn studio templates, folder structure for ui, blocks and features, components.json aliases, the cn helper in lib/utils, and Turborepo monorepos with a shared ui package. Use when starting a project or deciding where components live.
- **setup** — Set up shadcn/ui and shadcn studio in a Next.js project. This skill should be used when the user asks to "set up shadcn", "install shadcn/ui", "initialize shadcn", "configure shadcn studio", "add shadcn to my project", "set up component library", "init shadcn in next.js", or needs to initialize a Next.js project for shadcn/ui component development.

- **theme-configuration** — Theme shadcn/ui and shadcn studio projects — OKLCH CSS variables with @theme inline, light and dark variants, next-themes setup, next/font fonts, brand colors, style presets applied with shadcn apply, and installing shadcn studio themes. Use when changing colors, fonts or dark mode of a shadcn project.
- **troubleshoot** — Diagnose shadcn/ui and shadcn studio problems — components.json errors, Tailwind v3 or v4 mismatches, a missing cn helper after CLI 4.21, unstyled components or unset CSS variables, studio registry 401 or 403, dependency conflicts with React 19, hydration issues with next-themes. Use when an install, init or render fails.
- **add-registries** — Fetch the shadcn registries from the official endpoint (skipping unavailable and hidden ones) and add them to components.json
- **search-components** — Search across the shadcn registry directory (400+ registries) for UI components, blocks, and templates
- **setup-registries** — Set up community shadcn registries, the shadcn MCP server, and CLAUDE.md section for a project

## Subagents

Codex does not install plugin subagents automatically — copy them manually before use:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- **nextjs-provisioner** — Next.js UI provisioner for setting up component libraries, themes, and project architecture with shadcn/ui and shadcn studio.

<example>
Context: User wants to set up shadcn/ui in their Next.js project
user: "Set up shadcn/ui with shadcn studio premium components in my Next.js project"
assistant: "I'll use the nextjs-provisioner agent to initialize shadcn/ui, configure studio registries, and install core components."
<commentary>
User needs full shadcn setup including studio registries — agent verifies prerequisites, initializes shadcn, and configures premium registry access.
</commentary>
</example>

<example>
Context: User wants to add a complete dashboard UI
user: "I need a dashboard layout with sidebar, header, data tables, and charts using shadcn components"
assistant: "I'll use the nextjs-provisioner agent to install the required components and scaffold the dashboard layout."
<commentary>
User needs multiple components installed and composed into a layout — agent selects appropriate components/blocks from the registry and installs them.
</commentary>
</example>

<example>
Context: User wants to customize the theme
user: "Set up a dark blue theme with custom brand colors for my shadcn components"
assistant: "I'll use the nextjs-provisioner agent to configure the theme CSS variables and set up dark mode."
<commentary>
User needs theme customization — agent generates CSS custom properties, configures dark mode, and applies the theme.
</commentary>
</example>

<example>
Context: User wants animated components from community registries
user: "I need some cool animated components for my landing page — shimmer buttons, animated beams, parallax scroll"
assistant: "I'll use the nextjs-provisioner agent to search community registries for animation components and install them."
<commentary>
User needs specialty components not in the standard shadcn/ui registry — agent searches community registries (MagicUI, Aceternity, COSS…) and installs matching components.
</commentary>
</example>


## MCP servers

Add to `~/.codex/config.toml`:

```toml
[mcp_servers.shadcn]
command = "npx"
args = ["shadcn@latest","mcp"]

```
