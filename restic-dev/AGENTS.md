# restic-dev

> restic backup plugin for Agents Store. Set up encrypted daily backups on any Linux server to S3-compatible storage (Cloudflare R2): server recon + restic install, auto-discovery of all Docker volumes/mounts and databases, R2 repository init, a partial-failure-tolerant backup script with logical DB dumps and retention, timezone-aware systemd/cron scheduling, verification, monitoring/dead-man's-switch, and disaster recovery. File-based knowledge, no MCP, no stored credentials.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/restic-dev

## Skills

- **backup-script** — This skill should be used when the user asks to "write the restic backup script", "add database dumps to my backup", "set restic retention/forget policy", "create excludes for restic", or needs the daily script that dumps databases, backs up the discovered paths, tolerates exit code 3, and prunes old snapshots.
- **cli-reference** — This skill should be used when the user asks for the "restic command reference", "all restic commands", "restic flags", "restic environment variables", "restic exit codes", or needs the full command/flag/env-var reference for restic.
- **disaster-recovery** — This skill should be used when the user asks to "restore from a restic backup", "recover Docker data or a database from restic", "rebuild my server from backups", "do a full or partial restic restore", "my server died how do I get my data back", or needs to restore files, replay database dumps, and stand services back up.
- **discover-backup-sources** — This skill should be used when the user asks to "figure out what to back up", "find all my Docker volumes", "discover backup sources", "what should I be backing up on this server", "scan my Docker projects for backup", or points at a projects folder and wants the plugin to inspect every container's mounts and databases and build a concrete backup plan.
- **examples** — This skill should be used when the user asks for a "restic backup example", "end-to-end restic R2 walkthrough", "Docker server backup tutorial", "restic disaster recovery example", "how would this work on my server", or wants a complete scenario walkthrough.
- **monitoring** — This skill should be used when the user asks to "monitor restic backups", "get alerted when a backup fails", "set up a healthcheck or dead-man's-switch for backups", "detect stale restic snapshots", "alert me if backups stop", or needs ongoing backup health monitoring.
- **repository-setup** — This skill should be used when the user asks to "set up a restic repository on Cloudflare R2", "configure restic with S3 credentials", "create the restic encryption password", "initialize a restic repo", "fix restic AccessDenied on R2", or needs to wire up the password, R2/S3 env file, repository URL, and run restic init.
- **scheduling** — This skill should be used when the user asks to "schedule daily restic backups", "create a systemd timer for restic", "set up a cron job for backups", "run my backup at a specific time/timezone", or needs a timezone-aware schedule (systemd timer, or cron fallback on non-systemd hosts).
- **setup** — This skill should be used when the user asks to "set up restic backups on a server", "install restic", "prepare a Linux server for backups", "check what arch/init system my server uses", or needs to recon a server (architecture, OS, init system, timezone, free disk) and install the correct latest restic binary before configuring backups.
- **troubleshoot** — This skill should be used when the user hits "restic errors", "restic backup fails in cron", "restic repository is locked", "restic AccessDenied on R2", "restic wrong password", "restic SignatureDoesNotMatch", or needs to diagnose restic exit codes, S3/R2 errors, locks, repo/index/cache problems, and cron/systemd-only failures.
- **verify-backup** — This skill should be used when the user asks to "verify my restic backup works", "test a restic restore", "check restic snapshots and integrity", "validate backups before enabling the schedule", or needs to run the first backup, confirm snapshots, run restic check, test-restore, then enable the timer.
- **backup-now** — Run an ad-hoc restic backup now and show the resulting snapshot
- **restore** — Guided restore / disaster recovery from a restic repository
- **status** — Show restic backup health — timer state, latest snapshot, freshness, recent log

## Subagents

Codex does not install plugin subagents automatically — copy them manually before use:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- **restic-backup-engineer** — Use this agent when the user needs to set up, operate, or recover encrypted restic backups on a Linux server — reconning the host, auto-discovering Docker volumes and databases to back up, configuring a Cloudflare R2 / S3 repository, writing and scheduling a verified daily backup, monitoring it, or restoring after a disaster.

<example>
Context: Fresh Linux box with Docker projects and R2 credentials; wants offsite daily backups.
user: "I've got a bunch of Docker projects in /docker and Cloudflare R2 credentials. Set up automated daily backups."
assistant: "I'll use the restic-backup-engineer agent to recon the server, discover all Docker volumes and databases, configure the R2 repository, and schedule verified daily backups."
<commentary>End-to-end provisioning: recon → discover → repo → script → schedule → verify. The agent's core flow.</commentary>
</example>

<example>
Context: User is unsure where their data actually lives.
user: "I think my data is in /docker but I'm not 100% sure where everything is. What should I be backing up?"
assistant: "I'll use the restic-backup-engineer agent to auto-discover every container's mounts and databases — including data outside /docker — and produce a concrete backup plan for your review."
<commentary>discover-backup-sources: mapping containers→working_dir→mounts→DBs even outside the named folder, then confirming before writing anything.</commentary>
</example>

<example>
Context: A server died; user must restore onto a new box.
user: "My server is gone. I have the restic password and R2 keys. How do I get my Postgres data and Docker volumes back on a new server?"
assistant: "I'll use the restic-backup-engineer agent to rebuild on the new server — install restic, reconnect the R2 repo, restore volumes, and replay the database dumps."
<commentary>disaster-recovery / rebuild: restore files + DB-dump replay on fresh infrastructure.</commentary>
</example>

