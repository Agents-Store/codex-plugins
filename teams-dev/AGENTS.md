# teams-dev

> Microsoft Teams SDK dev plugin for Agents Store. TypeScript guidance for building Teams bots, message extensions, tabs, dialogs and AI agents on Teams SDK 2.1 and Teams Developer CLI 3. Vendors the official microsoft/teams-sdk skill (scaffold, bot registration, SSO setup) and adds skills for the App framework and turn state, Adaptive Cards, openai/MCP/A2A agents, Microsoft Graph, SSO with addOAuthFlow, deployment, sovereign clouds and the Agents Playground.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/teams-dev

## Skills

- **adaptive-cards** — Use this skill when the user is building or sending Adaptive Cards from a Microsoft Teams bot on `@microsoft/teams.cards` 2.1 — `AdaptiveCard`, `TextBlock`, inputs with `.withId()`, `ExecuteAction` with `SubmitData`, routing `card.action.<action>`, input validation, pasting Designer JSON, search-as-you-type choice sets. Triggers on "Teams card", "Adaptive Card", "Action.Execute", "card form", "card.action", "Adaptive Card Designer".
- **agents-playground** — Use this skill when the user wants to test or debug a Microsoft Teams bot locally on Teams SDK 2.1 — Microsoft 365 Agents Playground (`agentsplayground`), accepting unauthenticated local requests (`dangerouslyAllowUnauthenticatedRequests`), the "No credentials configured … all incoming requests will be rejected" warning, mock activities, logs, replacing the deprecated DevTools. Triggers on "Agents Playground", "test Teams bot locally", "No credentials configured", "dangerouslyAllowUnauthenticatedRequests", "Teams DevTools replacement".
- **ai-agents** — Use this skill when the user is adding LLM behaviour to a Microsoft Teams bot on Teams SDK 2.1 — calling OpenAI or Azure OpenAI directly with the `openai` SDK and `runTools()`, streaming into Teams, tools as `RunnableToolFunction`, conversation history in turn state, clarification cards, AI labels, feedback buttons, citations, suggested prompts. Triggers on "AI Teams bot", "OpenAI Teams", "function calling Teams", "streaming Teams response", "runTools", "Azure OpenAI Teams".
- **api-reference** — Use this skill on explicit request when the user asks for the Teams SDK 2.1 API reference — the `@microsoft/teams.*` packages and what each exports, `App` options, the handler context, activity routes, the `app.api` clients. Reference-only — does not auto-load.
- **authentication** — Use this skill when the user is wiring authentication into a Microsoft Teams app on Teams SDK 2.1 — app credentials (`CLIENT_ID`/`CLIENT_SECRET`/`TENANT_ID`, managed identity), user sign-in with `app.addOAuthFlow()` (SSO or third-party OAuth), several connections, sign-in callbacks, Nested App Authentication for tabs. Triggers on "Teams SSO", "addOAuthFlow", "OAuth Teams bot", "signIn", "onSignInComplete", "NAA Teams", "managed identity Teams bot".
- **cli-recipes** — Use this skill when the user is driving the Microsoft Teams Developer CLI 3.x (`teams`) from a script, a CI job or an agent — `teams login`, `teams project new typescript`, `teams app create|get|update|doctor`, manifest and RSC edits, `--json` output with jq, non-interactive flags. Triggers on "teams CLI", "teams app update", "teams app create", "teams app doctor", "teams manifest", "teams self-update", "teams status".
- **deployment** — Use this skill when the user is deploying a Microsoft Teams bot built on Teams SDK 2.1 — runtime environment variables (`CLIENT_ID`, `CLIENT_SECRET`, `TENANT_ID`, `MANAGED_IDENTITY_CLIENT_ID`), managed identity and federated credentials, the messaging endpoint, install and sideloading, organisation rollout, production hosting, secret rotation, sovereign clouds (`CLOUD`, US Gov, China). Triggers on "deploy Teams bot", "sideload Teams app", "Teams app production", "Teams sovereign cloud", "Teams bot endpoint", "managed identity Teams bot".
- **dialogs** — Use this skill when the user is building dialogs (task modules) in a Microsoft Teams bot on Teams SDK 2.1 — opening a dialog from a card with `OpenDialogData`, handling `dialog.open.<id>` and `dialog.submit.<action>`, returning an Adaptive Card or a web page, multi-step flows, dialogs opened from message extensions. Triggers on "Teams dialog", "task module", "modal in Teams", "dialog.open", "dialog.submit", "OpenDialogData".
- **examples** — Use this skill when the user wants a worked, end-to-end scenario for a Microsoft Teams app on Teams SDK 2.1 — a full walkthrough for an echo bot, an AI quote agent, an Adaptive Card form, a message-extension search command, or an SSO + Microsoft Graph bot. Triggers on "Teams bot example", "full example Teams", "end-to-end Teams scenario", "echo bot example", "Teams SSO example".
- **graph-integration** — Use this skill when the user is calling Microsoft Graph from a Microsoft Teams app on Teams SDK 2.1 — `app.graph.call(endpoints…)` as the app, a `GraphClient` built from a user's OAuth token, `@microsoft/teams.graph-endpoints` parameters, beta endpoints, paging, permissions. Triggers on "Microsoft Graph Teams", "graph-endpoints", "app.graph", "list calendar events bot", "send mail Teams bot", "graph permissions".
- **mcp-a2a** — Use this skill when the user is combining the Model Context Protocol or Agent2Agent with a Microsoft Teams bot on Teams SDK 2.1 — using a remote MCP server's tools in the agent, exposing the Teams bot as an MCP server (`find_user`, `notify`, `ask`, `request_approval`) with `@modelcontextprotocol/sdk`, bot-to-bot hand-off over A2A with `@a2a-js/sdk`. Triggers on "Teams MCP", "MCP server Teams", "expose Teams as MCP", "MCP client Teams bot", "A2A Teams", "human in the loop Teams".
- **message-extensions** — Use this skill when the user is building Microsoft Teams message extensions on Teams SDK 2.1 — search commands (`message.ext.query`), action commands (`message.ext.submit`, `message.ext.open`), link unfurling (`message.ext.query-link`), item selection, settings pages. Triggers on "Teams message extension", "compose extension", "link unfurling", "message.ext.query", "search command Teams", "action command".
- **messaging** — Use this skill when the user is sending or receiving messages in a Microsoft Teams bot on Teams SDK 2.1 — handling incoming messages, `send`/`reply`/`quote`, `MessageActivityInput`, typing indicators, mentions, targeted (private) messages, quoted replies, streaming and its limits, proactive notifications, receiving files, reactions. Triggers on "Teams bot message", "send a reply", "typing indicator", "stream a response", "proactive message", "targeted message", "quoted reply".
- **sdk-patterns** — Use this skill when the user is writing the core wiring of a Microsoft Teams app on Teams SDK 2.1 — booting `App`, credentials from the environment, per-turn state (`state`), activity routing (`app.on`, `app.message`, `app.event`), middleware (`app.use`, `next`), plugins, logging, or running the app on an existing HTTP server with an adapter. Triggers on "Teams App class", "Teams middleware", "register a plugin", "activity routing", "turn state", "httpServerAdapter".
- **tabs** — Use this skill when the user is building a tab inside a Microsoft Teams app on Teams SDK 2.1 — serving a static page with `app.tab()`, calling the agent from the page with `@microsoft/teams.client` and `app.function()`, using TeamsJS (`@microsoft/teams-js`) for context and dialogs, acquiring tokens with Nested App Authentication (NAA) and MSAL. Triggers on "Teams tab", "static tab", "configurable tab", "app.function", "teams.client", "Nested App Authentication", "TeamsJS".
- **teams-sdk-teams-dev** — Use this skill whenever the user mentions Microsoft Teams in a development context — whether they're building, integrating, configuring, debugging, or just asking questions. Covers bots, message extensions, embedded web apps, Adaptive Cards, dialogs, SSO, infrastructure, the Teams Developer CLI/SDK, and general Teams platform queries. If the word "Teams" appears, err on the side of invoking this skill.
- **troubleshoot** — Use this skill when the user is debugging a Microsoft Teams bot built on Teams SDK 2.1 — "bot did not reply", "All incoming requests will be rejected", 401/403 from or to Teams, sideload and install errors, SSO sign-in failures, Adaptive Card or dialog problems, streaming and AI issues, manifest checks. Triggers on "Teams bot not responding", "No credentials configured", "Teams 401", "Teams 403", "sideload error", "Teams app manifest invalid", "teams app doctor".
- **add-feature** — Add a feature (message handler, Adaptive Card, dialog, message extension, tab, AI agent, MCP server or client, A2A, SSO, Graph call, turn state) to an existing Microsoft Teams app on Teams SDK 2.1
- **scaffold** — Scaffold a new Microsoft Teams bot, Graph bot or tab on Teams SDK 2.1 with the Teams Developer CLI 3

## Subagents

Codex does not install plugin subagents automatically — copy them manually before use:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- **teams-developer** — Use this agent when the user needs help building a Microsoft Teams application in TypeScript or JavaScript with the Teams SDK 2.1 (`@microsoft/teams.*`) and the Teams Developer CLI 3 — bots, message extensions, tabs, dialogs, Adaptive Cards, AI agents on the `openai` SDK, MCP and A2A, Microsoft Graph calls, SSO with `addOAuthFlow`, bot registration, sideloading, sovereign clouds, or debugging with the Agents Playground.

<example>
Context: User wants a brand-new Teams bot.
user: "Build me a Teams bot that echoes messages and shows a button on every reply"
assistant: "I'll use the teams-developer agent to scaffold the bot with `teams project new typescript`, register it following the official teams-sdk-teams-dev guide, and attach an Adaptive Card whose `ExecuteAction` carries `SubmitData`."
<commentary>
Greenfield Teams bot — the agent picks the CLI template, follows the vendored registration guide and uses the 2.x card builders.
</commentary>
</example>

<example>
Context: User has an existing bot and needs Microsoft Graph access on behalf of the user.
user: "Add SSO to my existing Teams bot so I can list the signed-in user's calendar events"
assistant: "I'll use the teams-developer agent: migrate the bot to Azure if it is Teams-managed, run the SSO guide of teams-sdk-teams-dev, then add `app.addOAuthFlow('graph')` and a `GraphClient` built from the flow's token."
<commentary>
Multi-step auth change touching Azure, the manifest and code — the agent routes through the official SSO guide and the authentication and graph-integration skills.
</commentary>
</example>

<example>
Context: User wants an AI-powered Teams agent.
user: "Add an OpenAI-powered quote agent to my Teams app — stream the answer"
assistant: "I'll use the teams-developer agent to wire the `openai` SDK with `runTools()`, forward `content` deltas to `stream.emit()` in 1:1 chats, and keep the history in turn state."
<commentary>
AI integration spans ai-agents (model loop) and messaging (streaming limits). There are no Teams SDK AI packages any more.
</commentary>
</example>

