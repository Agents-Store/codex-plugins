# teams-dev

> Microsoft Teams SDK dev plugin for Agents Store. TypeScript-first guidance for building Teams bots, message extensions, tabs, dialogs, and AI agents using @microsoft/teams.* packages — covers the App framework, activity routing, Adaptive Cards, AI/MCP/A2A, Microsoft Graph, SSO, sovereign clouds, the Teams CLI, devtools, sideloading, and end-to-end scenarios.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/teams-dev

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **adaptive-cards** — Use this skill when the user is building or sending Adaptive Cards from a Microsoft Teams bot in TypeScript — using `AdaptiveCard`, `TextBlock`, `TextInput`, `ActionSet`, `Action.Submit`, `Action.Execute`, attaching cards via `cardAttachment`, or pasting JSON from the Adaptive Card Designer into TS builders. Triggers on "Teams card", "Adaptive Card", "Action.Submit", "card form".
- **ai-agents** — Use this skill when the user is adding LLM-powered behavior to a Microsoft Teams bot — building a `ChatPrompt`, attaching `OpenAIChatModel` (OpenAI or Azure OpenAI), registering function tools, streaming responses, or composing multi-agent flows with `A2AClientPlugin`. Triggers on "AI Teams bot", "ChatPrompt", "OpenAI Teams", "function calling Teams", "streaming Teams response", "A2A agent".
- **api-reference** — Use this skill on explicit request when the user asks for "Teams SDK API reference", "@microsoft/teams.* packages", "Teams App class options", "TS activity types", or needs precise signatures for `App`, `ChatPrompt`, `AdaptiveCard`, or other SDK types. Reference-only — does not auto-load.
- **authentication** — Use this skill when the user is wiring authentication into a Microsoft Teams app — app-only bot auth, user OAuth/SSO via `signin.token-exchange` and `signin.verify-state`, Nested App Authentication (NAA) for tabs, or MSAL token acquisition for calling Microsoft Graph. Triggers on "Teams SSO", "signin.token-exchange", "AAD app Teams", "NAA Teams", "OAuth Teams bot".
- **cli-recipes** — Use this skill when the user is invoking the Microsoft Teams Developer CLI (`teams`) — scaffolding projects with `teams project new`, listing or updating apps with `teams app list/get/update`, checking auth status, or upgrading the CLI itself. Triggers on "teams CLI", "teams project new", "teams app update", "teams self-update", "teams status".
- **deployment** — Use this skill when the user is deploying a Microsoft Teams bot — sideloading the manifest, registering bot infrastructure with the Teams CLI, switching between localhost / devtunnel / production hosts, configuring sovereign clouds (US Gov, GCC High, China), or planning a Teams app release. Triggers on "deploy Teams bot", "sideload Teams app", "Teams app production", "Teams sovereign cloud", "Teams bot endpoint".
- **devtools** — Use this skill when the user is inspecting a Microsoft Teams bot locally — using `DevtoolsPlugin`, the activity inspector at `http://localhost:3979/devtools`, viewing MCP requests, or debugging incoming activities and outgoing responses. Triggers on "Teams DevTools", "DevtoolsPlugin", "localhost:3979", "activity inspector Teams", "Teams MCP inspector".
- **dialogs** — Use this skill when the user is building task module dialogs in Microsoft Teams — opening a modal from an Adaptive Card button, handling `dialog.open.<id>` and `dialog.submit.<id>`, returning a card or a web page as the dialog body, or chaining a multi-step dialog flow. Triggers on "Teams dialog", "task module", "modal in Teams", "dialog.open", "dialog.submit".
- **examples** — Use this skill when the user wants a worked, end-to-end scenario for a Microsoft Teams app — a full walkthrough for an echo bot, an AI quote agent, an Adaptive Card form, a message-extension search command, or an SSO + Microsoft Graph bot. Triggers on "Teams bot example", "full example Teams", "end-to-end Teams scenario", "echo bot example", "Teams SSO example".
- **getting-started** — Use this skill when the user wants to scaffold their first Microsoft Teams app or bot project, asks "how do I start a Teams bot in TypeScript", or needs to run a fresh `teams project new` template end-to-end — from CLI scaffold to a working echo bot reachable from a Teams client.
- **graph-integration** — Use this skill when the user is calling Microsoft Graph from a Microsoft Teams bot — listing or creating users / events / messages / files via `api.graph`, switching between app-only and on-behalf-of user tokens, or designing a Graph-driven feature. Triggers on "Microsoft Graph Teams", "api.graph", "list calendar events bot", "send mail Teams bot", "graph permissions".
- **mcp-plugin** — Use this skill when the user is integrating the Model Context Protocol with a Microsoft Teams app — exposing the Teams bot's tools as an MCP server with `McpPlugin`, consuming a remote MCP server from inside the bot, configuring `@microsoft/teams.mcp`, or wiring an A2A flow. Triggers on "Teams MCP", "MCP plugin Teams", "expose tools as MCP", "McpPlugin", "A2AClientPlugin".
- **message-extensions** — Use this skill when the user is building Microsoft Teams message extensions — search-based "compose extension" commands, action commands, link unfurling, or item selection from a search list. Triggers on "Teams message extension", "compose extension", "link unfurling", "message.ext.query", "search command Teams".
- **messaging** — Use this skill when the user is sending or receiving messages in a Microsoft Teams bot — handling incoming messages, replying, sending typing indicators, mentioning users, replying in threads, streaming AI responses, or pushing proactive notifications. Triggers on "Teams bot message", "send a reply", "typing indicator", "stream a response", "proactive message".
- **sdk-patterns** — Use this skill when the user is writing the core wiring of a Microsoft Teams app — booting the `App`, registering plugins, structuring activity routes, chaining middleware with `next()`, or organising the project layout for a TypeScript Teams bot or app. Triggers on phrases like "Teams App class", "Teams middleware", "register a plugin", "activity routing".
- **setup** — Use this skill when the user is preparing a development environment for Microsoft Teams app development — installing the Teams Developer CLI, verifying Node version, confirming a Teams sideloading-enabled tenant, or troubleshooting "command not found" / "teams CLI not installed" / "no Microsoft 365 dev tenant" errors before any other Teams work begins.
- **tabs** — Use this skill when the user is building a static-hosted tab inside a Microsoft Teams app — serving a web page from the bot's server via `app.tab()`, using the `@microsoft/teams.client` SDK to call back into Teams, configuring tab context, or wiring a personal / channel / dialog tab. Triggers on "Teams tab", "static tab", "configurable tab", "Teams client SDK".
- **troubleshoot** — Use this skill when the user is debugging a Microsoft Teams app problem — sideload failures, "bot did not reply", SSO `signin.token-exchange` errors, manifest validation, endpoint mismatches, Adaptive Card rendering issues, or any "why isn't my Teams bot working" question. Triggers on "Teams bot not responding", "sideload error", "Teams 401", "Teams 403", "Teams app manifest invalid".

## Subagents

Defined under `.codex/agents/` as TOML files:

- **teams-developer** — Use this agent when the user needs help building a Microsoft Teams application in TypeScript or JavaScript with the official `@microsoft/teams.*` SDK — bots, message extensions, tabs, dialogs, Adaptive Cards, AI agents, MCP/A2A integrations, Microsoft Graph calls, SSO, sideloading, sovereign-cloud configuration, or debugging the `teams` CLI / DevTools workflow.

<example>
Context: User wants a brand-new Teams bot.
user: "Build me a Teams bot that echoes messages and shows a button on every reply"
assistant: "I'll use the teams-developer agent to scaffold the bot with the teams CLI, wire up an `app.on('message', …)` handler, and attach an Adaptive Card with an `Action.Submit` button."
<commentary>
Greenfield Teams bot — agent picks the right CLI template, app handler pattern, and card builder.
</commentary>
</example>

<example>
Context: User has an existing bot and needs Microsoft Graph access on behalf of the user.
user: "Add SSO to my existing Teams bot so I can list the signed-in user's calendar events"
assistant: "I'll use the teams-developer agent to walk through the SSO migration: AAD app registration, OAuth connection, manifest update, and a `signin.token-exchange` handler that calls `api.graph.me.events.list()`."
<commentary>
Multi-step auth change touching code, manifest, and Azure config — agent routes through the authentication and graph-integration skills.
</commentary>
</example>

<example>
Context: User wants an AI-powered Teams agent.
user: "Add an OpenAI-powered quote agent to my Teams app — when the user asks, reply with a quote and stream the answer"
assistant: "I'll use the teams-developer agent to wire up `ChatPrompt` with `OpenAIChatModel`, register the prompt inside `app.on('message', …)`, and stream chunks via `stream.emit()` for 1:1 chats."
<commentary>
AI integration spans the ai-agents skill (prompt/model) and messaging skill (streaming semantics).
</commentary>
</example>


## Workflows (canonical slash commands)

Codex CLI doesn't support custom slash commands — invoke these workflows via natural language. Each entry below is a prompt template you can adapt:

### `add-feature`

Add a feature (message handler, Adaptive Card, dialog, message extension, tab, AI agent, MCP plugin, SSO, Graph call) to an existing Microsoft Teams app

Arguments: `<feature>`

<details><summary>Prompt template</summary>

# /teams-dev:add-feature

Adds an incremental feature to an existing Microsoft Teams app built on `@microsoft/teams.*`. The argument selects the feature to add.

## Feature router

Match `$ARGUMENTS` (case-insensitive, hyphens and spaces interchangeable):

| Argument | Skill to invoke | Outcome |
|---|---|---|
| `message`, `message-handler`, `bot` | `messaging` | New `app.on('message', …)` handler with reply/typing patterns |
| `card`, `adaptive-card` | `adaptive-cards` | Build a card with `AdaptiveCard` + `cardAttachment('adaptive', card)` |
| `dialog`, `task-module` | `dialogs` | `dialog.open.<id>` + `dialog.submit.<id>` handlers |
| `message-extension`, `compose-extension`, `me` | `message-extensions` | `message.ext.query` and/or `submit` handlers + manifest changes |
| `tab`, `static-tab` | `tabs` | `app.tab('name', path)` and client-side SDK wiring |
| `ai`, `ai-agent`, `chat-prompt` | `ai-agents` | `ChatPrompt` + `OpenAIChatModel` and message-handler integration |
| `mcp`, `mcp-server`, `mcp-client` | `mcp-plugin` | `@microsoft/teams.mcp` server registration or client config |
| `sso`, `auth`, `signin` | `authentication` | Token-exchange + verify-state handlers, manifest scope update |
| `graph`, `microsoft-graph` | `graph-integration` | `api.graph.*` call with user-or-app token |

If no argument matches, ask the user which of these to add — do not guess.

## Steps

1. Glob the project: `Read` `package.json`, `src/index.ts` (or `src/main.ts`), and `appPackage/manifest.json`.
2. Invoke the matching skill from the table.
3. Propose a minimal diff. Do not refactor untouched code.
4. After applying the diff, run `npx tsc --noEmit` to catch type errors before suggesting `npm run dev`.

## Notes

- Always re-read `src/index.ts` before editing — the user may have customised `new App({ … })` options.
- For features that touch the manifest (`bot`, `message-extension`, `tab`, `sso`), update `appPackage/manifest.json` and remind the user that sideloading must be re-run with `teams app update`.
- Streaming responses only work in 1:1 chats; in channels/groups, emit the full response at the end. Flag this when adding `ai-agent`.

</details>

### `scaffold`

Scaffold a new Microsoft Teams bot, tab, AI agent, or Graph app with the Teams Developer CLI

Arguments: `<project-name> [template?]`

<details><summary>Prompt template</summary>

# /teams-dev:scaffold

Bootstraps a new Microsoft Teams application in the current working directory using `@microsoft/teams.cli`. Args:

- `$ARG1` — project folder name (required), e.g. `quote-agent`
- `$ARG2` — template (optional, default `echo`). Allowed: `echo`, `ai`, `graph`, `tab`.

## Steps

1. Invoke `Skill: setup` to confirm Node 18+ and a Teams developer account.
2. If `teams --version` fails, run `npm i -g @microsoft/teams.cli@preview`.
3. Run `teams project new typescript "$ARG1" --template "${ARG2:-echo}"`.
4. Cd into the new directory and `npm install`.
5. Open `src/index.ts`, `appPackage/manifest.json`, and `.env` to confirm scaffolding.
6. Read `Skill: getting-started` for next steps (run `npm run dev`, set up a devtunnel, sideload into Teams).

## Notes

- Template `echo` is the simplest message-echo bot; pick it unless the user asked otherwise.
- Template `ai` wires up `@microsoft/teams.ai` with a `ChatPrompt` + `OpenAIChatModel` — needs `OPENAI_API_KEY` in `.env`.
- Template `graph` wires up `api.graph` calls with user-token auth — needs SSO setup; route through `Skill: authentication` before testing.
- Template `tab` scaffolds a static-hosted React tab — pair with `Skill: tabs` for client-side SDK usage.
- Do not commit `.env` or `appPackage/manifest.json` rewrites that contain real `botId` values.

</details>
