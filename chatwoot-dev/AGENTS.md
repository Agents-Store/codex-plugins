# chatwoot-dev

> Chatwoot dev plugin for Agents Store. Full REST API coverage (Application, Platform, and Public/Client APIs) with bundled OpenAPI specs, official chatwoot CLI recipes, webhook & agent-bot automation, and troubleshooting for developers building on Chatwoot. Authenticates with the api_access_token header via CHATWOOT_API_KEY against CHATWOOT_BASE_URL.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/chatwoot-dev

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **api-reference** — This skill should be used when the user asks for "Chatwoot API endpoints", "Chatwoot REST API", "Chatwoot curl examples", "Chatwoot Application/Platform/Public API", "Chatwoot API documentation", or needs specific HTTP endpoint, request, or response details for Chatwoot.
- **cli-recipes** — This skill should be used when the user asks about the "Chatwoot CLI", "chatwoot command line", "chatwoot convs/conv/contact commands", "triage Chatwoot from the terminal", "run Chatwoot in CI", or needs ready-to-use `chatwoot` CLI commands, the noun/verb grammar, output-format rules, or safety guidance for customer-visible writes.
- **examples** — This skill should be used when the user wants a worked, end-to-end Chatwoot example — "show me a full Chatwoot integration", "seed a conversation via the API", "build a Chatwoot bot example", "bulk import contacts", or "triage Chatwoot from the terminal" — combining setup, the REST API, the CLI, and webhooks into a complete scenario.
- **setup** — This skill should be used when the user asks to "set up Chatwoot API", "get a Chatwoot access token", "configure CHATWOOT_API_KEY", "install the Chatwoot CLI", "connect to Chatwoot", "verify Chatwoot is working", or needs to obtain credentials and confirm REST API / CLI access to a Chatwoot account (cloud or self-hosted).
- **troubleshoot** — This skill should be used when the user hits "Chatwoot API errors", "Chatwoot 401/403/404", "Chatwoot CLI not working", "Chatwoot webhook signature mismatch", "Chatwoot token not authorized", or needs to diagnose and fix problems with the Chatwoot API, CLI, or webhooks.
- **webhooks-automation** — This skill should be used when the user wants to "set up a Chatwoot webhook", "verify a Chatwoot webhook signature", "build a Chatwoot agent bot", "handle Chatwoot events", "create an automation rule", or build event-driven integrations and bots on top of Chatwoot (webhooks, automation rules, agent bots, integration hooks).

## Subagents

Defined under `.codex/agents/` as TOML files:

- **chatwoot-assistant** — Use this agent when the user needs help building with or operating Chatwoot — writing REST API integration code, choosing the right API family (Application / Platform / Public), debugging api_access_token auth, building agent bots and webhook handlers, automating conversation routing, or scripting the chatwoot CLI.

<example>
Context: User is writing an integration against the Chatwoot API.
user: "Write a function that fetches all open conversations in inbox 5 and posts a summary reply."
assistant: "I'll use the chatwoot-assistant agent to build the integration and pick the right endpoints/auth."
<commentary>Developer needs Chatwoot API integration code with correct auth and pagination.</commentary>
</example>

<example>
Context: User is building a bot.
user: "Help me build a Chatwoot agent bot that auto-replies to new messages and verifies the webhook signature."
assistant: "I'll use the chatwoot-assistant agent to wire up the webhook receiver, signature check, and reply loop."
<commentary>Bot + webhook work spanning webhooks-automation and the Application API.</commentary>
</example>

<example>
Context: User wants to triage from the terminal.
user: "Show me how to find and resolve all spam-labelled conversations with the chatwoot CLI."
assistant: "I'll use the chatwoot-assistant agent to script the CLI triage safely."
<commentary>CLI scripting where customer-visible writes must be confirmed.</commentary>
</example>


## Workflows (canonical slash commands)

Codex CLI doesn't support custom slash commands — invoke these workflows via natural language. Each entry below is a prompt template you can adapt:

### `api-call`

Build (and optionally run) an authenticated Chatwoot REST API request — picks the correct API family and auth header, reads GETs freely, and confirms before any write.

Arguments: `resource or action,e.g. "list open conversations in inbox 5"`

<details><summary>Prompt template</summary>

Construct a correct, authenticated Chatwoot API request for: **$ARGUMENTS**

Follow these steps:

1. **Pick the API family** from the request:
   - Account/agent work (conversations, messages, contacts, inboxes, teams, reports,
     automation, webhooks, help center) → **Application API**
     `${CHATWOOT_BASE_URL}/api/v1/accounts/${CHATWOOT_ACCOUNT_ID}/...`
   - Installation provisioning (accounts, users, account-users, agent bots) → **Platform API**
     `${CHATWOOT_BASE_URL}/platform/api/v1/...` (uses the platform app token)
   - Customer/widget-facing → **Public/Client API**
     `${CHATWOOT_BASE_URL}/public/api/v1/inboxes/{inbox_identifier}/...` (no token)

2. **Find the exact endpoint** in the bundled specs before composing the request — do not
   guess paths or field names:
   ```bash
   D="${CLAUDE_PLUGIN_ROOT}/skills/api-reference/references/openapi"
   jq -r '.paths | keys[]' "$D/application_swagger.json" | grep -i <keyword>
   jq '.paths["<path>"]' "$D/application_swagger.json"
   ```
   The human-readable endpoint tables are in
   `${CLAUDE_PLUGIN_ROOT}/skills/api-reference/references/{application,platform,client}-api.md`.

3. **Build the curl** with the right auth header (`api_access_token`, never Bearer):
   ```bash
   curl -s -H "api_access_token: ${CHATWOOT_API_KEY}" \
     "${CHATWOOT_BASE_URL}/api/v1/accounts/${CHATWOOT_ACCOUNT_ID}/<path>" | jq .
   ```

4. **Run vs. confirm:**
   - If the request is a **GET** (read-only), you may run it.
   - If it is a **POST / PATCH / PUT / DELETE**, or any operation that sends a message or
     changes shared state, **show the exact command and ask for explicit confirmation before
     running it**. A sent message cannot be unsent.

5. If credentials are missing, point the user to the `setup` skill rather than inventing
   values. Never print or hardcode the token — reference `${CHATWOOT_API_KEY}`.

</details>

### `troubleshoot`

Diagnose a Chatwoot API / CLI / webhook problem — run read-only connectivity, auth, and scope checks against the configured instance and report the fix.

Arguments: `[object Object]`

<details><summary>Prompt template</summary>

Diagnose this Chatwoot problem: **$ARGUMENTS**

Run the read-only checks below (all GETs — safe), interpret the results, and recommend a fix.
Load the `troubleshoot` skill for the full symptom → cause → fix tables.

1. **Env vars present?**
   ```bash
   for v in CHATWOOT_BASE_URL CHATWOOT_API_KEY CHATWOOT_ACCOUNT_ID; do
     [ -n "${!v}" ] && echo "$v: set" || echo "$v: MISSING"
   done
   ```
   If any are missing, stop and point the user to the `setup` skill.

2. **Base URL reachable?**
   ```bash
   curl -sI "${CHATWOOT_BASE_URL}" | head -1
   ```

3. **Token valid + identity?** (401 ⇒ bad token or wrong header; remember it's
   `api_access_token`, not `Authorization: Bearer`)
   ```bash
   curl -s -o /dev/null -w "profile HTTP %{http_code}\n" \
     -H "api_access_token: ${CHATWOOT_API_KEY}" "${CHATWOOT_BASE_URL}/api/v1/profile"
   curl -s -H "api_access_token: ${CHATWOOT_API_KEY}" \
     "${CHATWOOT_BASE_URL}/api/v1/profile" | jq '{id, name, role}' 2>/dev/null
   ```

4. **Account scope resolves?** (404 ⇒ wrong `account_id` or base URL)
   ```bash
   curl -s -o /dev/null -w "conversations HTTP %{http_code}\n" \
     -H "api_access_token: ${CHATWOOT_API_KEY}" \
     "${CHATWOOT_BASE_URL}/api/v1/accounts/${CHATWOOT_ACCOUNT_ID}/conversations"
   ```

5. **CLI installed/authenticated?** (optional)
   ```bash
   command -v chatwoot >/dev/null && chatwoot auth status 2>&1 | head -5 || echo "chatwoot CLI not installed"
   ```

6. Map the observed status codes to the fix using the `troubleshoot` skill:
   401 → token/header or wrong API family · 403 → insufficient role · 404 → account_id/base
   URL · 422 → request body · 429 → rate limit/backoff. For webhook signature mismatches,
   verify the HMAC is computed over the **raw** body as `"{timestamp}.{raw_body}"`.

Report what passed, what failed, and the single most likely fix.

</details>
