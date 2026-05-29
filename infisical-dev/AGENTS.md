# infisical-dev

> Infisical CLI dev plugin for Agents Store. Complete command-line coverage for secrets management — install & auth, infisical run/secrets/export, dynamic secrets, secret scanning with pre-commit hooks, machine-identity CI/CD auth, self-hosted, and troubleshooting.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/infisical-dev

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **ci-cd-auth** — This skill should be used when the user asks to "use Infisical in CI/CD", "Infisical machine identity", "infisical login universal-auth", "authenticate Infisical without a browser", "Infisical in Docker", "inject secrets in a pipeline", "Infisical Kubernetes/AWS/GCP/Azure auth", or "bootstrap a self-hosted Infisical" — non-interactive Infisical CLI authentication and secret injection for automation.
- **cli-recipes** — This skill should be used when the user asks about "Infisical run", "infisical secrets", "inject secrets into a process", "Infisical export to .env", "infisical CLI commands", "manage secrets from the terminal", "Infisical folders", or "Infisical dynamic secrets lease" — the everyday Infisical CLI workflows for fetching, setting, injecting, and exporting secrets.
- **cli-reference** — This skill should be used when the user asks for "Infisical CLI reference", "all Infisical commands", "Infisical CLI flags", "Infisical environment variables", "infisical command list", or needs the full command/flag/env-var reference for the Infisical CLI.
- **secret-scanning** — This skill should be used when the user asks to "scan for secrets", "Infisical scan", "find leaked secrets in git", "set up a pre-commit secret scan", "infisical scan git-changes", "scan repo for API keys", "add a secret-scanning baseline", or "ignore a false-positive secret" — using the Infisical CLI to detect and prevent committed secrets.
- **setup** — This skill should be used when the user asks to "install Infisical CLI", "set up Infisical CLI", "log in to Infisical", "authenticate Infisical CLI", "verify Infisical is working", "is Infisical CLI working", "point Infisical at self-hosted", or needs to get the Infisical CLI installed, authenticated, and confirmed operational.
- **troubleshoot** — This skill should be used when the user hits "Infisical CLI errors", "Infisical login fails", "Infisical keyring error", "infisical project not found", "Infisical token not working", "Infisical self-hosted connection issues", "infisical run no secrets", or needs to diagnose and fix problems with the Infisical CLI.

## Subagents

Defined under `.codex/agents/` as TOML files:

- **infisical-developer** — Use this agent when the user needs help working with the Infisical CLI for secrets management — wiring `infisical run` into a dev or build workflow, setting up machine-identity auth for CI/CD, configuring secret scanning and pre-commit hooks, scripting secret import/export, or debugging Infisical CLI errors.

<example>
Context: User wants to stop committing a local .env and inject secrets at runtime instead.
user: "We keep a .env in the repo for our Node app — how do I switch to Infisical so secrets aren't on disk?"
assistant: "I'll use the infisical-developer agent to convert the workflow to `infisical run`."
<commentary>Developer needs a CLI-based secret-injection workflow — the agent's core domain.</commentary>
</example>

<example>
Context: User is wiring Infisical into a GitHub Actions pipeline.
user: "My GitHub Action needs prod secrets from Infisical but there's no browser to log in. How do I authenticate?"
assistant: "I'll use the infisical-developer agent to set up machine-identity auth in CI."
<commentary>Non-interactive machine-identity auth and pipeline injection — the agent knows the login/token/run pattern.</commentary>
</example>

<example>
Context: User wants to block secrets from being committed.
user: "Can Infisical scan our repo for leaked API keys and add a pre-commit check?"
assistant: "I'll use the infisical-developer agent to set up `infisical scan` and the pre-commit hook."
<commentary>Secret scanning and hook setup — covered by the agent's scanning knowledge.</commentary>
</example>

