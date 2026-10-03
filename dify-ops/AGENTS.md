# dify-ops

> Dify self-hosted update operations plugin. Pre-flight the target release and the bundled Weaviate migration path, back up volumes with the stack down, merge a release tag into the local dev branch, sync .env variables, and pull and restart containers for Dify Docker deployments.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/dify-ops

## Skills

- **dify-docker-architecture** — Dify Docker Compose deployment architecture — services, container naming, directory layout, .env.example and envs/ structure, and Docker project name conventions. Use when working with Dify Docker setup, understanding container services, debugging container issues, or needing to know the Dify directory structure. Triggers on "dify docker", "dify containers", "dify services", "dify architecture", "dify compose".

- **env-sync** — Synchronize .env with .env.example for Dify Docker deployments, including the optional envs/ templates — detect new, removed and changed variables, add missing ones with default values, preserve existing customizations, never print secrets. Use when syncing env variables, checking for new Dify configuration variables, comparing .env.example vs .env, "env sync", "new env variables", "missing environment variables", or after pulling Dify updates.

- **examples** — End-to-end scenario walkthroughs for updating self-hosted Dify. Use when the user asks for "dify update example", "how to update dify", "show me an update walkthrough", "dify upgrade guide", "step-by-step dify update", or needs a complete example of the update process.

- **update-workflow** — Git and backup workflow for updating self-hosted Dify — pick the target tag, pre-flight the bundled Weaviate migration path, back up volumes with the stack down, merge into dev, handle conflicts, pull images, verify, roll back. Use when updating Dify, merging upstream changes, handling merge conflicts in Dify, backing up before an update, or switching to a specific Dify version/tag. Triggers on "update dify", "pull dify changes", "merge main into dev", "upgrade dify", "dify version", "dify backup", "weaviate upgrade".

- **status** — Show current Dify instance status — git branch, running version vs latest stable tag, container health, .env sync state
- **update** — Update self-hosted Dify — pre-flight (Weaviate migration gate), back up volumes with the stack down, merge a release tag into dev, sync env, pull images, verify

## Subagents

Codex does not install plugin subagents automatically — copy them manually before use:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- **dify-updater** — Dify self-hosted update operations agent. Handles updating Dify Docker deployments — checking the
target release (including the bundled Weaviate migration path), backing up volumes with the stack
down, merging a release tag into the local dev branch, syncing environment variables, detecting
Docker project names, and pulling and restarting containers.

<example>
Context: User wants to update to latest version
user: "Update my Dify to the latest version"
assistant: "I'll check your current Dify state, resolve the latest stable tag and run the pre-flight before changing anything."
<commentary>
Standard update request — agent runs the full update workflow: pre-flight (Weaviate gate), backup with the stack down, merge, env sync, pull and start.
</commentary>
</example>

<example>
Context: User wants a specific version
user: "Update Dify to version 1.17.1"
assistant: "I'll fetch tags, run the pre-flight for 1.17.1 and, once the backup exists, merge it into your dev branch."
<commentary>
Tagged release update — agent verifies the tag exists (tags have no v prefix), stops at the Weaviate gate when it applies, backs up, merges into dev, handles conflicts.
</commentary>
</example>

<example>
Context: User wants to check status
user: "Check if my Dify needs updating"
assistant: "I'll compare the version you run with the latest stable release and report the status."
<commentary>
Read-only check — agent compares the running image tag with the latest stable tag and reports, including whether the bundled Weaviate needs a staged upgrade.
</commentary>
</example>

<example>
Context: User has merge conflicts
user: "I have merge conflicts after updating Dify"
assistant: "I'll look at the conflicted files and help you resolve them."
<commentary>
Conflict resolution — agent lists conflicted files, shows diffs, recommends resolution strategy.
</commentary>
</example>

<example>
Context: User wants env sync only
user: "Sync my Dify .env with the latest .env.example"
assistant: "I'll compare your .env against .env.example and the envs/ templates and add any missing variables."
<commentary>
Standalone env sync — agent runs the official sync script through a masking filter, checks the keys a name comparison misses, and appends missing variables after confirmation.
</commentary>
</example>

