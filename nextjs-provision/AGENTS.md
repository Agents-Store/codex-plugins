# nextjs-provision

> Next.js provisioning plugin. Set up shadcn/ui and shadcn studio — component installation, theme configuration, MCP server setup, project scaffolding, and multi-registry component search across 30+ free community registries.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/nextjs-provision

## MCP servers

Add to `~/.codex/config.toml`:

```toml
[mcp_servers.shadcn]
command = "npx"
args = ["shadcn@latest","mcp"]

[mcp_servers.shadcn-community]
command = "npx"
args = ["-y","@jpisnice/shadcn-ui-mcp-server"]

```

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **component-registry** — Browse, search, install, and use shadcn/ui and shadcn studio components, blocks, and templates. This skill should be used when the user asks to "install a shadcn component", "add a button", "list shadcn blocks", "find a form block", "browse shadcn studio components", "add a card component", "install a navigation block", "what components are available", "write code with shadcn components", "use Button component", "render shadcn component as link", "Button as a link", "shadcn component patterns", or needs to discover, install, or use UI components from shadcn registries correctly.

- **component-search** — Search and install UI components from 30+ free community shadcn registries. This skill should be used when the user asks to "search for shadcn components", "find a calendar component", "browse community registries", "install from magicui", "what shadcn registries are available", "add animated components", "search for a date picker", "find UI blocks for landing page", "install from aceternity", "what community components exist", or needs to discover and install components from community registries beyond the standard shadcn/ui and shadcn studio registries.

- **examples** — End-to-end scenario walkthroughs for setting up Next.js projects with shadcn/ui and shadcn studio. This skill should be used when the user asks for "shadcn setup walkthrough", "how to set up a project with shadcn from scratch", "add shadcn to existing project example", "full shadcn setup guide", "shadcn studio tutorial", "step-by-step shadcn setup", or needs a complete example of provisioning a Next.js project with shadcn components.

- **mcp-tools** — Set up and use shadcn MCP servers for AI-assisted component discovery and installation. This skill should be used when the user asks about "shadcn MCP", "shadcn MCP server", "set up shadcn MCP for Claude", "component MCP tools", "Jpisnice shadcn MCP", "shadcn-ui-mcp-server", "AI component installation", or needs to configure MCP servers for shadcn/ui component work.

- **project-scaffolding** — Scaffold Next.js projects with shadcn/ui templates, starter kits, and component architecture patterns. This skill should be used when the user asks to "scaffold a project", "use a shadcn template", "project structure with shadcn", "component organization", "set up a dashboard template", "starter kit for shadcn", "how to organize shadcn components", or needs guidance on organizing a Next.js project with shadcn/ui component architecture.

- **setup** — Set up shadcn/ui and shadcn studio in a Next.js project. This skill should be used when the user asks to "set up shadcn", "install shadcn/ui", "initialize shadcn", "configure shadcn studio", "add shadcn to my project", "set up component library", "init shadcn in next.js", or needs to initialize a Next.js project for shadcn/ui component development.

- **theme-configuration** — Configure themes, CSS variables, colors, fonts, and dark mode for shadcn/ui and shadcn studio. This skill should be used when the user asks to "set up a theme", "customize shadcn colors", "add dark mode", "change shadcn theme", "use shadcn theme generator", "configure CSS variables for shadcn", "install a shadcn studio theme", "customize fonts", "brand colors for shadcn", or needs to design and apply visual themes to their shadcn/ui project.

- **troubleshoot** — Diagnose and fix common shadcn/ui and shadcn studio setup issues, dependency conflicts, and configuration problems. This skill should be used when the user encounters "shadcn install error", "components.json error", "tailwind not working with shadcn", "shadcn component not rendering", "CSS variables not applied", "shadcn studio registry error", "dependency conflict", "cn() not found", or needs to debug problems with their shadcn/ui setup.


## Subagents

Defined under `.codex/agents/` as TOML files:

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
User needs specialty components not in the standard shadcn/ui registry — agent searches community registries (MagicUI, Aceternity UI) and installs matching components.
</commentary>
</example>


## Workflows (canonical slash commands)

Codex CLI doesn't support custom slash commands — invoke these workflows via natural language. Each entry below is a prompt template you can adapt:

### `add-registries`

Fetch all 180+ shadcn registries from the official endpoint and add them to components.json

Arguments: `--filter <keyword>`

<details><summary>Prompt template</summary>

# Add Registries

Fetch the complete list of shadcn-compatible registries from the official endpoint and populate the project's `components.json`.

## Instructions

1. Verify `components.json` exists in the project root. If not, ask whether to run `npx shadcn@latest init` first.

2. Fetch the official registry list:
   ```bash
   curl -s https://ui.shadcn.com/r/registries.json
   ```
   Or use WebFetch on `https://ui.shadcn.com/r/registries.json`.

3. Parse the JSON response. Each entry has:
   - `name` — e.g. `"@magicui"`
   - `url` — e.g. `"https://magicui.design/r/{name}.json"`
   - `description` — brief text

4. If `$ARGUMENTS` contains `--filter <keyword>`, only include registries whose `name` or `description` contains the keyword (case-insensitive).

5. Read the current `components.json` and extract the existing `"registries"` object (may be empty or missing).

6. Build the new registries object by merging existing entries with the fetched ones. For each fetched registry:
   - Key: the `name` field (e.g. `"@magicui"`)
   - Value: the `url` field (e.g. `"https://magicui.design/r/{name}.json"`)

7. Write the merged `"registries"` back to `components.json`. Preserve all other fields.

8. Report: how many registries were added (new) vs already present (skipped).

## Example Output

```
Fetched 180 registries from https://ui.shadcn.com/r/registries.json
Added 175 new registries to components.json
Skipped 5 already configured
Total registries in components.json: 178
```

</details>

### `search-components`

Search across 30+ free community shadcn registries for UI components, blocks, and templates

Arguments: `<what-you-need>`

<details><summary>Prompt template</summary>

# Search Components

Search for shadcn-compatible components across 30+ community registries.

## Instructions

1. Read the skill at `${CLAUDE_PLUGIN_ROOT}/skills/component-search/SKILL.md`
2. Read the registry reference at `${CLAUDE_PLUGIN_ROOT}/skills/component-search/references/community-registries.md`
3. Parse the user's search query from "$ARGUMENTS" (e.g., "animated button", "date range picker", "chat component", "pricing section")
4. Identify the most relevant category: animation, extended UI, blocks, e-commerce, AI, file upload, other
5. Present matching registries and components with install commands:

```
## Results for "$ARGUMENTS"

### Recommended registries:
- **@registryname** — description
  Install: `npx shadcn@latest add @registryname/component`

### Also check:
- **@registryname2** — description
```

6. Check if the user's project has `components.json` — if registries are not configured, suggest running `/setup-registries` first
7. If MCP servers are available (`shadcn` or `shadcn-community`), use them to search for more specific matches

</details>

### `setup-registries`

Set up community shadcn registries, MCP servers, and CLAUDE.md section for a project

Arguments: `--mcp | --claudemd | --skill`

<details><summary>Prompt template</summary>

# Setup Registries

Full project setup for shadcn community registry search — registries, MCP, CLAUDE.md section, and official skill.

## Instructions

1. Read the skill at `${CLAUDE_PLUGIN_ROOT}/skills/component-search/SKILL.md`

2. Verify `components.json` exists in the project root. If not:
   - Ask the user if they want to initialize shadcn/ui first
   - Run `npx shadcn@latest init` if approved

3. Parse arguments from "$ARGUMENTS":
   - (no args) — full setup: registries + MCP + CLAUDE.md + skill
   - `--mcp` — only configure MCP servers
   - `--claudemd` — only add CLAUDE.md section
   - `--skill` — only install official shadcn skill

4. **Add registries** — Run `/add-registries` to fetch all 180+ registries from `https://ui.shadcn.com/r/registries.json` and add them to `components.json`

5. **Configure MCP servers** — Show the template from `${CLAUDE_PLUGIN_ROOT}/skills/component-search/references/mcp-config-template.json` and create/update the project's `.mcp.json`

6. **Install the official shadcn skill**:
   ```bash
   pnpm dlx skills add shadcn/ui
   ```

7. **Add CLAUDE.md section** — Append the content from `${CLAUDE_PLUGIN_ROOT}/skills/component-search/references/claude-md-section.md` to the project's CLAUDE.md

8. Test by installing one component from a community registry to verify it works

</details>
