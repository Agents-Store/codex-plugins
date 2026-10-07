# vaultwarden-dev

> Vaultwarden dev plugin for Agents Store. Script and integrate a self-hosted Vaultwarden (Bitwarden-compatible) server: what the client API can and cannot do under end-to-end encryption, the identity token flows, organization member management, the /admin panel API, the Bitwarden CLI (bw, bw serve), python-vaultwarden and Terraform, client/server version compatibility, troubleshooting, and a guard hook that asks before a command prints decrypted secrets into the chat. File-based knowledge, no MCP, no stored credentials.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/vaultwarden-dev

## Skills

- **admin-panel** — This skill should be used when the user asks to "use the Vaultwarden admin API", "script the /admin panel", "disable or delete a Vaultwarden user", "reset a user's 2FA", "change Vaultwarden config", or mentions ADMIN_TOKEN or the VW_ADMIN cookie.
- **api-reference** — Manual reference (invoke /vaultwarden-dev:api-reference) for "Vaultwarden API endpoints", "Vaultwarden REST API routes", "Vaultwarden token grants", "Vaultwarden curl examples" — exact HTTP routes, payloads and auth of a Vaultwarden server.
- **cli-recipes** — This skill should be used when the user asks to "use bw with Vaultwarden", "get a password from Vaultwarden in a script", "create a Vaultwarden item from the CLI", "confirm org members with bw", "run bw serve", or needs Bitwarden CLI recipes for a self-hosted Vaultwarden.
- **examples** — This skill should be used when the user asks "how do I onboard/offboard someone in Vaultwarden", "show a Vaultwarden CI pipeline example", "back up and upgrade Vaultwarden", or wants an end-to-end walkthrough combining the admin panel, client API and bw CLI.
- **mcp-patterns** — This skill should be used when the user asks for "a Vaultwarden MCP server", "connect Claude to my Vaultwarden", "use the Bitwarden MCP server with Vaultwarden", "warden-mcp", or wants an agent to query a self-hosted vault through MCP tools instead of shell commands.
- **sdk-patterns** — This skill should be used when the user asks for a "Vaultwarden SDK" or "Bitwarden SDK for Vaultwarden", to "automate Vaultwarden from Python", "use python-vaultwarden", "manage Vaultwarden with Terraform", or wants a library rather than curl/bw for a self-hosted Vaultwarden.
- **secret-hygiene** — This skill should be used when a Vaultwarden or bw task could print, log or store a secret — "store BW_SESSION", "show me the password from Vaultwarden", "export the vault in plain text", "put the master password in a script" — to keep secrets out of chat, logs, git and argv.
- **setup** — This skill should be used when the user asks to "connect to Vaultwarden", "point bw at my Vaultwarden server", "check Vaultwarden is reachable", "get a Vaultwarden API token", "call the Vaultwarden REST API", "manage org members via the API", or "sync users from LDAP into Vaultwarden".
- **troubleshoot** — This skill should be used when the user reports Vaultwarden errors — "Username or password is incorrect" with the right password, "404 on user-key-id", "Vaultwarden 401 or 429", "invites not arriving", "bw sync hangs" — or needs to diagnose a self-hosted Vaultwarden or bw failure.
- **check-version** — Check that a Vaultwarden server is reachable and that the local Bitwarden CLI version is compatible with it (read-only)

## Subagents

Codex does not install plugin subagents automatically — copy them manually before use:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- **vaultwarden-developer** — Use this agent when the user needs hands-on help building against a self-hosted Vaultwarden (Bitwarden-compatible) server — writing scripts or services that use the Bitwarden CLI, `bw serve`, the client API, the /admin panel API, python-vaultwarden or the Terraform provider, or debugging such an integration.

<example>
Context: User wants a script that provisions access for new hires
user: "Write a Python script that invites a list of emails to our Vaultwarden org and gives them read access to the Infra collection"
assistant: "I'll use the vaultwarden-developer agent to build the onboarding script."
<commentary>
Integration code against Vaultwarden's org API with the python-vaultwarden library — the agent knows which steps need client crypto (confirmation) and which do not.
</commentary>
</example>

<example>
Context: User's CI job stopped fetching secrets
user: "Our pipeline's bw login started failing with a 404 on user-key-id after we updated the runner image"
assistant: "I'll use the vaultwarden-developer agent to diagnose the CLI/server version mismatch."
<commentary>
Debugging a Bitwarden CLI against Vaultwarden — a known version-coupling failure the agent can verify and fix.
</commentary>
</example>

<example>
Context: User is designing how services get their secrets
user: "How should our three backend services read their database passwords from Vaultwarden without anyone hardcoding them?"
assistant: "I'll use the vaultwarden-developer agent to design the secret-delivery pattern."
<commentary>
Architecture question about bw / bw serve / dedicated accounts and collections — the agent proposes a design that respects end-to-end encryption and secret hygiene.
</commentary>
</example>


## Hooks

This plugin ships `hooks/hooks.json`. After installing, run `/hooks` to trust it before its hooks will run. Note: `PreToolUse.additionalContext` is currently ignored by Codex.
