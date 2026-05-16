# openclaw-configurator

> OpenClaw instance configurator and operations plugin. Scan, analyze, and optimize all workspace files (AGENTS.md, SOUL.md, USER.md, IDENTITY.md, TOOLS.md, HEARTBEAT.md, MEMORY.md, BOOT.md, BOOTSTRAP.md) plus openclaw.json. Update instances from official GitHub releases. Guided interviews, session log analysis, standing orders design, security audit, config validation, permission fix hooks, and 6 industry-specific workspace templates.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/openclaw-configurator

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **agents-md** — Guide for creating and customizing AGENTS.md — the operating rules, procedures, and behavioral guardrails for an OpenClaw agent. Use this skill whenever the user is working on agent behavior, operating procedures, session startup sequences, memory management policies, group chat rules, safety red lines, tool usage priorities, or standing order references. Even if they just say "the agent should do X differently" or "add a rule for Y", this skill applies because operational rules belong in AGENTS.md. Also use when deciding what belongs in AGENTS.md versus SOUL.md — this skill clarifies the boundary.
- **bootstrap-boot** — Guide for BOOTSTRAP.md (first-run setup ritual) and BOOT.md (gateway restart checklist) in OpenClaw. Use this skill whenever the user is setting up a new agent for the first time, configuring onboarding flows, creating first-run discovery processes, or defining what should happen on gateway restart. Even questions like "what should happen when the agent starts for the first time", "how do I initialize a fresh agent", or "what runs on reboot" need this skill.
- **config-validation** — Validates openclaw.json against official OpenClaw documentation and checks for latest features, deprecated settings, and security issues. Use this skill whenever the user wants to verify their configuration is correct, check if they're using the latest OpenClaw features, audit their openclaw.json for problems, or compare their config against best practices. Also applies when the user says things like "is my config OK", "what am I missing in my setup", or "check my OpenClaw configuration".
- **examples** — Complete end-to-end workspace customization examples for different industries — legal firms, dev teams, marketing agencies, customer support, personal assistants, and content creators. Use this skill whenever the user wants to see a reference implementation, asks "how would you set up OpenClaw for X", wants to see what a complete workspace looks like, or needs a starting template for their industry. Even vague requests like "show me an example" or "what does a good workspace look like" should trigger this skill.
- **heartbeat-md** — Guide for configuring HEARTBEAT.md — the periodic background task checklist that OpenClaw executes on a schedule. Use this skill whenever the user needs background monitoring, periodic checks, standing orders integration, quiet hours configuration, or wants to understand heartbeat mechanics and token costs. Even questions like "how do I make the agent check something periodically", "set up monitoring", or "reduce heartbeat token costs" need this skill.
- **identity-md** — Guide for creating IDENTITY.md — the agent's name, creature type, vibe, emoji, and avatar in OpenClaw. Use this skill whenever the user is naming their agent, choosing an emoji or avatar, setting up the agent's visual identity, or asking how agent identity works. Even simple questions like "what should I name my agent", "how do I set the bot emoji", or "change the agent's avatar" need this skill.
- **memory-system** — Guide for OpenClaw's memory system — MEMORY.md curation, daily logs, memory flush, and vector search. Use this skill whenever the user asks about how the agent remembers things between sessions, wants to configure memory management, needs to understand daily logs versus long-term memory, or is setting up memory curation processes. Also applies to questions about vector search, memory security in groups, or "how does the agent remember".
- **openclaw-config** — Comprehensive guide for openclaw.json — the central gateway configuration file controlling models, channels, tools, plugins, and sessions. Use this skill whenever the user needs to understand, modify, or troubleshoot their openclaw.json. Covers agent settings, Telegram/Discord/WhatsApp channel setup, tool profiles, plugin management, session behavior, secret handling with SecretRef, and model configuration. Even questions like "how do I add Telegram", "change the model", or "what does this config field do" need this skill.
- **security-audit** — Workspace prompt security audit checklist — checks for hardcoded secrets, prompt injection risks, data leakage, missing safety rules, PII exposure, and overly broad standing orders. Use this skill whenever auditing workspace security, checking for vulnerabilities, reviewing prompt safety, or as part of any workspace optimization. Even if the user doesn't explicitly ask for security, include these checks when doing a full workspace review, optimization, or pre-deployment audit.
- **session-analysis** — Techniques for analyzing OpenClaw session JSONL logs to understand user behavior, tool effectiveness, error patterns, token costs, and active hours. Use this skill whenever the user wants to analyze how their agent is performing, what users are asking, which tools work best, what errors occur, or how to improve the workspace based on real usage data. Also applies to questions like "what do my users ask about", "is my agent working well", or "show me session stats".
- **soul-md** — Guide for creating and refining SOUL.md — the agent's personality, values, tone, and boundaries. Use this skill whenever the user is working on who the agent should be, its communication style, persona traits, behavioral boundaries, or domain-specific character. Even requests like "make the agent more friendly", "add a boundary", "change the tone", or "the agent sounds too corporate" need this skill because personality and tone live in SOUL.md, not AGENTS.md.
- **standing-orders** — Design patterns for standing orders — autonomous programs that give agents permanent operating authority for defined tasks. Use this skill whenever the user needs recurring tasks, scheduled agent operations, autonomous execution rules, or the execute-verify-report pattern. Also applies when discussing cron jobs with OpenClaw, periodic reports, automated monitoring, content scheduling, or any task the agent should do on its own without prompting each time.
- **tools-md** — Guide for optimizing TOOLS.md — environment-specific tool notes, search priorities, device names, and local infrastructure details. Use this skill whenever the user is configuring which tools their agent should prefer, adding environment notes, documenting search tool priority orders, or setting up device/API references. Even questions like "which search tool should my agent use first", "how do I tell the agent about my devices", or "add tool notes" need this skill.
- **user-md** — Guide for building USER.md — user profiles, preferences, roles, and communication styles. Use this skill whenever the user is setting up who will interact with the agent, mapping Telegram/Discord user IDs to names, configuring multi-user access, or defining communication preferences. Even questions like "how does the agent know who I am", "add a team member", or "set up user permissions" need this skill.
- **workspace-overview** — OpenClaw workspace architecture and file system overview — directory structure, file loading mechanics, character limits, scan categories (A-J), and subfolder organization patterns. Use this skill whenever the user asks about how the workspace is structured, what files get loaded automatically, character limits, what goes where, or how to organize their workspace. This is the foundational skill — use it first when the user is new to OpenClaw or asks broad questions about workspace organization, even before more specific skills like agents-md or soul-md.

## Subagents

Defined under `.codex/agents/` as TOML files:

- **openclaw-configurator-assistant** — Interactive OpenClaw workspace and configuration assistant. Helps users scan, analyze, and optimize all workspace files (AGENTS.md, SOUL.md, USER.md, IDENTITY.md, TOOLS.md, HEARTBEAT.md, MEMORY.md, BOOTSTRAP.md, BOOT.md) plus openclaw.json. Guides through interviews, session analysis, security audits, config validation, and industry-specific workspace templates.

<example>
user: "Help me set up my OpenClaw workspace for a legal firm"
</example>
<example>
user: "Optimize my OpenClaw SOUL.md"
</example>
<example>
user: "Scan my OpenClaw workspace and tell me what's missing"
</example>
<example>
user: "How should I configure AGENTS.md for a dev team?"
</example>
<example>
user: "Analyze my OpenClaw session logs for optimization opportunities"
</example>
<example>
user: "Configure my openclaw.json channels"
</example>
<example>
user: "Validate my OpenClaw config against the latest docs"
</example>
<example>
user: "Run a security audit on my workspace"
</example>

- **openclaw-workspace-auditor** — Autonomous OpenClaw workspace auditor. Scans all workspace files (categories A–J), sessions, cron, logs, skills, plugins, openclaw.json, performs prompt security audit, checks for inline secrets, validates config against official docs, and produces a comprehensive health report. Read-only — does not modify files.

<example>
user: "Audit my OpenClaw workspace"
</example>
<example>
user: "Review my OpenClaw workspace health"
</example>
<example>
user: "What's wrong with my OpenClaw configuration?"
</example>
<example>
user: "Check my workspace for security issues"
</example>


## Workflows (canonical slash commands)

Codex CLI doesn't support custom slash commands — invoke these workflows via natural language. Each entry below is a prompt template you can adapt:

### `config-validate`

Validate openclaw.json against official documentation, check for latest features, detect inline secrets, and verify cross-references with workspace files

Arguments: `[--secrets] [--docs] [--all]`

<details><summary>Prompt template</summary>

# Config Validate

Validate the current OpenClaw instance's `openclaw.json` against official documentation and best practices. The active instance dir is `$OPENCLAW_PROJECT_DIR` when set, otherwise the current working directory (standard: `~/.openclaw/`, Docker multi-instance: `~/.openclaw-{name}/`).

## Process

### 1. Resolve and read openclaw.json

```bash
INSTANCE_DIR="${OPENCLAW_PROJECT_DIR:-$(pwd)}"
cd "$INSTANCE_DIR" || { echo "ERROR: cannot access $INSTANCE_DIR"; exit 1; }
cat ./openclaw.json
```

If not found, report error and stop.

### 2. Structure Validation

Check that all expected top-level sections exist:
- `agents` (required)
- `channels` (at least one channel recommended)
- `tools` (recommended)
- `plugins` (optional)
- `session` (optional)

### 3. Secret Detection

```bash
# Check for potential inline secrets (tokens, API keys)
grep -n -E '"[A-Za-z0-9_:.-]{20,}"' ./openclaw.json | grep -vi '"source"\|"provider"\|"id"\|"model"\|"profile"\|"mode"\|"workspace"\|"description"\|"name"'
```

If inline secrets found:
- Flag as CRITICAL security issue
- Show the correct SecretRef pattern:
  ```json
  {
    "source": "env",
    "provider": "default",
    "id": "ENV_VAR_NAME"
  }
  ```
- Remind user that `.env` file should contain the actual values

**Known false positive**: `openclaw doctor` may report `unresolved SecretRef "env:default:..."` when running outside gateway runtime. This is safe to ignore.

### 4. Field-Level Validation

Check each field against expected values. Use the **config-validation** skill for the complete checklist. Key checks:

- `agents.defaults.model` — is it set and valid?
- `agents.defaults.heartbeat.model` — using a cheap model?
- `channels.*.dmPolicy` — not `"open"` in production?
- `tools.loopDetection.enabled` — true for production?
- `agents.defaults.userTimezone` — valid IANA timezone?

### 5. Cross-Validation with Workspace Files

Read workspace files and cross-check:

```bash
# Check which workspace files exist
for f in AGENTS.md SOUL.md USER.md IDENTITY.md TOOLS.md HEARTBEAT.md MEMORY.md; do
  [ -f "./workspace/$f" ] && echo "OK $f" || echo "MISSING $f"
done
```

Cross-checks:
- Every user ID in `channels.*.allowFrom` should have a profile in `./workspace/USER.md`
- If heartbeat is configured, `./workspace/HEARTBEAT.md` should exist and have content
- If groups are configured, `./workspace/AGENTS.md` should have group chat rules
- `userTimezone` should match timezone in `./workspace/USER.md`

### 6. Fetch Latest Documentation

Use available search/scraping tools to check official OpenClaw docs for latest features:

**Tool priority** (use the best available):
1. Firecrawl tools — `firecrawl_scrape` for page content, `firecrawl_search` for queries
2. Exa.ai — `web_search_exa` for code-aware search
3. Perplexity — `search` for synthesis
4. Jina — `read_url` for page reading
5. WebFetch — basic URL fetch

**What to check**:
- `https://docs.openclaw.ai` — latest configuration reference
- `https://github.com/openclaw/openclaw` — changelog, new features

Compare current config with latest available features. Flag:
- New fields available but not configured
- Deprecated fields still in use
- Newer model versions available

### 7. Generate Validation Report

```markdown
# openclaw.json Validation Report

## Instance
- Path: [CWD]
- Config size: [bytes]

## Security
- Inline secrets: [NONE / FOUND (list)]
- SecretRef usage: [correct / needs migration]
- dmPolicy: [secure / needs review]

## Configuration Quality
| Section | Status | Issues |
|---------|--------|--------|
| agents.defaults | OK/WARN | [details] |
| heartbeat | OK/WARN/MISSING | [details] |
| channels | OK/WARN | [details] |
| tools | OK/WARN | [details] |
| plugins | OK/WARN/NONE | [details] |
| session | OK/WARN/NONE | [details] |

## Cross-Validation
| Check | Result |
|-------|--------|
| allowFrom ↔ USER.md | [match/mismatch] |
| heartbeat ↔ HEARTBEAT.md | [aligned/misaligned] |
| groups ↔ AGENTS.md | [covered/missing] |
| timezone ↔ USER.md | [match/mismatch] |

## Feature Freshness
- [New features available / all current]

## Recommendations
1. [Most important fix]
2. [Second priority]
3. [Third priority]
```

### 8. Offer to Fix

If issues found, offer to fix them:
- For openclaw.json changes: show proposed diff, ask for explicit confirmation
- Always back up before editing: `cp ./openclaw.json ./openclaw.json.bak`
- After editing: run `openclaw doctor --fix` to verify (Docker multi-instance: `openclaw-{name} doctor --fix`)
- For workspace file issues: suggest using `/workspace-optimize <file>`

</details>

### `instance-update`

Update OpenClaw instance from official GitHub repo — fetch latest tag, merge into dev preserving local changes, rebuild Docker containers

Arguments: `[tag-or-version]`

<details><summary>Prompt template</summary>

# Instance Update

Update an OpenClaw instance from the official GitHub repository. Fetches the latest release tag (or a specified tag), merges it into the local `dev` branch preserving local customizations, and rebuilds Docker containers.

## Arguments

- `$ARGUMENTS` — optional tag/version (e.g., `v1.5.0`, `1.5.0`). If empty, update to the latest tag.

## Process

### Step 1: Verify Working Directory

The command targets `$OPENCLAW_PROJECT_DIR` when set, otherwise the current directory (e.g., `/docker/openclaw-pitline/`). The target must be a git repo cloned from the official OpenClaw repo and contain `docker-compose.yaml`.

```bash
PROJECT_DIR="${OPENCLAW_PROJECT_DIR:-$(pwd)}"
cd "$PROJECT_DIR" || { echo "ERROR: cannot access $PROJECT_DIR"; exit 1; }

if [ ! -d ".git" ]; then
  echo "ERROR: Not a git repository. Run this from (or set OPENCLAW_PROJECT_DIR to) the OpenClaw project root (e.g., /docker/openclaw-pitline/)"
  exit 1
fi

if [ ! -f "docker-compose.yaml" ] && [ ! -f "docker-compose.yml" ]; then
  echo "ERROR: No docker-compose.yaml found in $PROJECT_DIR. Expected to be in the OpenClaw project root."
  exit 1
fi

COMPOSE_FILE=$([ -f "docker-compose.yaml" ] && echo "docker-compose.yaml" || echo "docker-compose.yml")
INSTANCE_NAME=$(basename "$PROJECT_DIR")
echo "Instance: $INSTANCE_NAME"
echo "Directory: $PROJECT_DIR"
echo "Compose file: $COMPOSE_FILE"
```

### Step 2: Pre-flight Checks

```bash
# Current branch
CURRENT_BRANCH=$(git branch --show-current)
echo "Branch: $CURRENT_BRANCH"

# Verify origin
ORIGIN_URL=$(git remote get-url origin 2>/dev/null)
echo "Origin: $ORIGIN_URL"

# Current position
PRE_UPDATE_COMMIT=$(git rev-parse --short HEAD)
PRE_UPDATE_TAG=$(git describe --tags --abbrev=0 2>/dev/null || echo "none")
echo "Current commit: $PRE_UPDATE_COMMIT"
echo "Current tag: $PRE_UPDATE_TAG"

# Uncommitted changes
git status --short
```

**Branch check**: If `CURRENT_BRANCH` is not `dev`, warn the user. Ask whether to switch to `dev` or abort. The `dev` branch is the local customization branch — all merges happen here.

**Origin check**: If `ORIGIN_URL` does not contain `openclaw/openclaw`, warn the user. Ask whether to continue or abort.

**Dirty working tree**: If `git status --short` shows changes, ask user:
1. **Commit changes first** — `git add -A && git commit -m "WIP: local changes before update to $TARGET_TAG"`
2. **Stash changes** — `git stash push -m "pre-update stash $(date +%Y-%m-%d-%H%M)"`
3. **Abort update**

If stashing, remember to restore in Step 11.

### Step 3: Backup docker-compose

```bash
cp "$COMPOSE_FILE" "${COMPOSE_FILE}.pre-update.bak"
echo "Backed up $COMPOSE_FILE → ${COMPOSE_FILE}.pre-update.bak"

# Also backup .env if present
[ -f ".env" ] && cp .env .env.pre-update.bak && echo "Backed up .env → .env.pre-update.bak"
```

### Step 4: Fetch Upstream

```bash
git fetch origin --tags
echo "Fetched latest from origin (including all tags)"
```

### Step 5: Determine Target Tag

**If `$ARGUMENTS` is empty (update to latest):**

```bash
LATEST_TAG=$(git tag --sort=-version:refname | head -1)
if [ -z "$LATEST_TAG" ]; then
  echo "ERROR: No tags found after fetch. Verify origin is correct."
  exit 1
fi
echo "Latest available tag: $LATEST_TAG"
```

If `PRE_UPDATE_TAG` equals `LATEST_TAG`:
```
Already on the latest tag ($LATEST_TAG). No update needed.
Recent tags:
$(git tag --sort=-version:refname | head -5)
```
Stop here — clean up backup files and exit.

**If `$ARGUMENTS` is provided:**

```bash
TAG="$ARGUMENTS"

# Try exact match, then with 'v' prefix
if ! git tag -l "$TAG" | grep -q .; then
  if git tag -l "v$TAG" | grep -q .; then
    TAG="v$TAG"
    echo "Found tag: $TAG (added v prefix)"
  else
    echo "ERROR: Tag '$TAG' not found after fetching."
    echo "Available recent tags:"
    git tag --sort=-version:refname | head -10
  fi
fi
```

If tag not found, stop and ask user to select a valid tag.

### Step 6: Show Update Plan and Confirm

```bash
TARGET_TAG="${TAG:-$LATEST_TAG}"

echo ""
echo "=== Update Plan ==="
echo "Instance:  $INSTANCE_NAME"
echo "Current:   $PRE_UPDATE_TAG ($PRE_UPDATE_COMMIT)"
echo "Target:    $TARGET_TAG"
echo ""
echo "Commits to merge:"
git log --oneline "$PRE_UPDATE_COMMIT..$TARGET_TAG" 2>/dev/null | head -20
COMMIT_COUNT=$(git rev-list --count "$PRE_UPDATE_COMMIT..$TARGET_TAG" 2>/dev/null || echo "?")
echo ""
echo "Total: $COMMIT_COUNT commits"
echo "Changelog: https://github.com/openclaw/openclaw/releases"
```

Ask user to confirm before proceeding with the merge.

### Step 7: Merge Tag into dev

```bash
git checkout dev
git merge "$TARGET_TAG" -m "Merge $TARGET_TAG into dev"
```

**If merge succeeds cleanly** — proceed to Step 8.

**If merge conflicts occur:**

```bash
echo "Merge conflicts detected:"
git diff --name-only --diff-filter=U
```

Resolution strategy — **preserve local customizations**:

| File pattern | Strategy | Reason |
|---|---|---|
| `docker-compose.yaml` / `docker-compose.yml` | **Manual merge** — show conflicts to user, preserve local ports/volumes/services, accept upstream additions | User's custom configuration lives here |
| `.env` / `.env.example` | Accept theirs, then restore custom values | Template should match upstream |
| Source code / other files | Accept theirs | We want upstream code changes |

For **docker-compose conflicts**:
1. Show the conflicted sections to the user
2. User's customizations (ports, volumes, environment overrides, added services) must be **kept**
3. Upstream changes (new services, updated image tags, new env vars) should be **accepted**
4. Help merge manually, then `git add $COMPOSE_FILE`

For **non-docker-compose conflicts**:
```bash
CONFLICTS=$(git diff --name-only --diff-filter=U)
for f in $CONFLICTS; do
  case "$f" in
    docker-compose.yaml|docker-compose.yml|.env)
      echo "MANUAL: $f — needs user review"
      ;;
    *)
      git checkout --theirs "$f"
      git add "$f"
      echo "AUTO: $f — accepted upstream version"
      ;;
  esac
done
```

After all conflicts resolved:
```bash
git commit -m "Merge $TARGET_TAG into dev — conflicts resolved"
```

**CRITICAL**: Never push the `dev` branch. This is the user's local-only customization branch.

### Step 8: Validate docker-compose

```bash
docker compose config --quiet 2>&1 && echo "docker-compose: valid syntax" || echo "WARNING: docker-compose has syntax errors — review before rebuilding"
```

If syntax errors detected, show the error and ask user to fix before proceeding. The backup is available at `${COMPOSE_FILE}.pre-update.bak`.

### Step 9: Rebuild Containers

Ask for confirmation before rebuilding.

```bash
echo "Rebuilding containers..."
docker compose up -d --build
```

If build fails:
1. Show the error output
2. Offer options:
   - Fix the issue and retry
   - **Rollback**: `git merge --abort` (if merge not committed) or `git reset --hard $PRE_UPDATE_COMMIT` then `docker compose up -d --build`

### Step 10: Verify Containers

```bash
sleep 10
echo "=== Container Status ==="
docker compose ps
```

If not all containers are running:
1. Show failed services: `docker compose ps | grep -v "running\|Up"`
2. Show logs: `docker compose logs --tail=50 <failed_service>`
3. Offer to retry or roll back

### Step 11: Restore Stashed Changes

If changes were stashed in Step 2:

```bash
git stash pop
echo "Restored stashed changes"
```

If stash pop has conflicts, show them and help resolve.

### Step 12: Summary

```
=== OpenClaw Instance Update Complete ===

Instance:         $INSTANCE_NAME ($PROJECT_DIR)
Previous version: $PRE_UPDATE_TAG ($PRE_UPDATE_COMMIT)
Updated to:       $TARGET_TAG ($(git rev-parse --short HEAD))
Branch:           dev (local only — NOT pushed)
Conflicts:        [None / N resolved]
Backups:          ${COMPOSE_FILE}.pre-update.bak
Containers:       [docker compose ps summary]

Rollback:  git reset --hard $PRE_UPDATE_COMMIT && docker compose up -d --build
Backup:    cp ${COMPOSE_FILE}.pre-update.bak $COMPOSE_FILE
Releases:  https://github.com/openclaw/openclaw/releases
```

Ask user if they want to keep or remove backup files:
```bash
rm -f "${COMPOSE_FILE}.pre-update.bak" .env.pre-update.bak
echo "Backup files removed"
```

</details>

### `workspace-interview`

Guided discovery session to understand goals, users, tasks, and success criteria for OpenClaw workspace customization

Arguments: `[new|existing]`

<details><summary>Prompt template</summary>

# Workspace Interview

Conduct a guided discovery session to understand what the OpenClaw workspace needs to achieve. CWD is the instance root (`~/.openclaw-{name}/`).

**Note**: The interview conversation can be in the user's preferred language. However, all generated workspace files and the workspace brief MUST be written in English.

## Process

### Step 1: Check current state
Before interviewing, scan the workspace to understand what already exists:
```bash
for f in AGENTS.md SOUL.md USER.md IDENTITY.md TOOLS.md HEARTBEAT.md MEMORY.md; do
  [ -f "./workspace/$f" ] && echo "EXISTS: $f ($(wc -w < "./workspace/$f") words)" || echo "MISSING: $f"
done
```

Also check `./openclaw.json` for existing channel and model configuration.

### Step 2: Interview — 7 question areas

Ask these questions conversationally, not as a rigid form. Adapt based on answers. Ask 2-3 at a time, not all at once.

**Area 1: Goals & Purpose**
- What is this OpenClaw agent for? (personal assistant, team tool, customer-facing, internal ops?)
- What's the single most important thing it should do well?
- What would success look like in 30 days?

**Area 2: Users**
- Who will interact with this agent? (one person, a team, clients?)
- What are their roles? (developer, manager, lawyer, etc.)
- What languages do they communicate in?
- What timezone(s)?

**Area 3: Regular Tasks**
- What tasks are assigned to the agent most often?
- Are there recurring/scheduled tasks?
- How complex are typical requests? (simple Q&A, multi-step workflows, research?)

**Area 4: Channels**
- Which platforms will the agent use? (Telegram, Discord, WhatsApp, web, iMessage?)
- Are there group chats? How should the agent behave in them?
- Should the agent be proactive (initiate messages) or reactive only?

**Area 5: Tools & Skills**
- What tools/skills does the agent need? (search, code, email, calendar, home automation?)
- Are there domain-specific tools? (legal databases, analytics platforms, CRM?)
- Any tools that should be prioritized or avoided?

**Area 6: Success Criteria**
- How will you know the agent is doing well?
- What are the must-not-fail scenarios?
- What are the red lines? (things the agent must never do)

**Area 7: Pain Points**
- If you already have a workspace: what's not working?
- What does the agent get wrong most often?
- What do you wish it would do differently?

### Step 3: Compile brief

After gathering all answers, create a structured brief **in English**:

```markdown
# Workspace Configuration Brief

## Instance Purpose
[Summary of goals and primary function]

## Users
| Name | Role | Language | Timezone | Permissions |
|------|------|----------|----------|-------------|
| ... | ... | ... | ... | ... |

## Primary Tasks
1. [Most important task]
2. [Second priority]
3. [Third priority]

## Channels
- [Channel 1]: [configuration needs]
- [Channel 2]: [configuration needs]

## Tools & Skills
- Priority: [tools to emphasize]
- Avoid: [tools to restrict or not use]

## Success Criteria
- [Metric 1]
- [Metric 2]

## Red Lines
- [Must never do 1]
- [Must never do 2]

## Pain Points (if existing workspace)
- [Issue 1]
- [Issue 2]

## Recommended Scenario Template
Based on this profile, closest match: [legal-firm / dev-team / personal-assistant / marketing-agency / customer-support / content-creator]
```

### Step 4: Present brief and next steps

Show the compiled brief to the user and suggest next steps:

1. Save brief to workspace as `./workspace/docs/workspace-brief.md`
2. Run `/workspace-optimize all` to generate all workspace files based on this brief
3. Or optimize specific files: `/workspace-optimize soul`, `/workspace-optimize agents`, etc.
4. Run `/config-validate` to check openclaw.json
5. After all edits: fix Docker permissions

Ask for approval before saving.

</details>

### `workspace-optimize`

Optimize a specific OpenClaw workspace file or all files at once, with security checks and optional openclaw.json editing

Arguments: `<soul|agents|user|tools|heartbeat|identity|memory|bootstrap|boot|config|all>`

<details><summary>Prompt template</summary>

# Workspace Optimize

Optimize a specific OpenClaw workspace file following best practices. CWD is the instance root (standard: `~/.openclaw/`, Docker multi-instance: `~/.openclaw-{name}/`). Files are read from and written to `./workspace/`.

**All generated content MUST be in English.** The interview/conversation with the user can be in any language, but workspace file content is always English.

## Arguments
- file-type: `soul`, `agents`, `user`, `tools`, `heartbeat`, `identity`, `memory`, `bootstrap`, `boot`, `config`, or `all` (required)

Parse from "$ARGUMENTS".

## Process

### 1. Parse arguments
Extract file type from arguments.

### 2. Read current file
Map file type to filename:
- `soul` → `./workspace/SOUL.md` (auto-injected every session)
- `agents` → `./workspace/AGENTS.md` (auto-injected every session)
- `user` → `./workspace/USER.md` (auto-injected every session)
- `tools` → `./workspace/TOOLS.md` (auto-injected every session)
- `heartbeat` → `./workspace/HEARTBEAT.md` (auto-injected in heartbeat runs)
- `identity` → `./workspace/IDENTITY.md` (auto-injected every session)
- `memory` → `./workspace/MEMORY.md` (auto-injected in main sessions only)
- `bootstrap` → `./workspace/BOOTSTRAP.md` (auto-injected when present — one-time first-run ritual, deleted after bootstrap completes)
- `boot` → `./workspace/BOOT.md` (NOT auto-injected — optional, executed via hook on gateway restart)
- `config` → `./openclaw.json` (editable with user permission)
- `all` → process all files sequentially

Read the current file. If it doesn't exist, note that we'll create it from scratch.

### 3. Load relevant skill
Load the corresponding skill for the file type:
- `soul` → soul-md skill
- `agents` → agents-md skill
- `user` → user-md skill
- `tools` → tools-md skill
- `heartbeat` → heartbeat-md skill
- `identity` → identity-md skill
- `memory` → memory-system skill
- `bootstrap` or `boot` → bootstrap-boot skill
- `config` → openclaw-config + config-validation skills

### 4. Gather context
Before optimizing, read related files for context:
- Other workspace files in `./workspace/`
- `./openclaw.json` (for channels, model, tools config)
- Session logs in `./agents/main/sessions/` if available
- Docs subfolders: `./workspace/docs/`, `./workspace/workflows/`
- Canvas: `./workspace/canvas/`

### 5. Security check
Before generating optimized content, run security checks:
- Scan current file for hardcoded secrets
- Check for PII that shouldn't be there
- Verify safety rules are present (red lines in AGENTS.md, boundaries in SOUL.md)
- For `config`: check SecretRef pattern usage

Use the **security-audit** skill for the full checklist.

### 6. Ask clarifying questions
If the current file is empty or being created:
- What is the purpose of this OpenClaw instance?
- Who are the primary users?
- What domain/industry?
- Any specific requirements?

### 7. Generate optimized version
Following the skill's best practices:
- Apply the correct template structure
- Include all recommended sections
- Ensure word count is within limits (SOUL.md < 2,000 words)
- Ensure character count is within limits (< 20,000 chars for auto-injected files)
- For large auto-injected files: recommend extracting infrequently-used content to `docs/` subfolders
- Maintain consistency with other workspace files
- **All content in English**
- Include security best practices (red lines, boundaries, memory isolation)

### 8. Show diff and apply
- Display the proposed new content
- If updating existing file: show what changed
- Ask for approval before writing
- Write the file to `./workspace/` with user's confirmation
- For `config`:
  1. Show proposed `./openclaw.json` changes as diff
  2. Ask for **explicit user confirmation**
  3. Back up: `cp ./openclaw.json ./openclaw.json.bak`
  4. Validate JSON syntax before writing
  5. Apply changes
  6. Run: `openclaw doctor --fix` (Docker multi-instance: `openclaw-{name} doctor --fix`)

### 9. Fix permissions (Docker deployments only)
After writing files in Docker deployments, fix permissions:
```bash
# Derive instance name from OPENCLAW_PROJECT_DIR (if set) or CWD (e.g., /root/.openclaw-team → team)
INSTANCE_DIR="${OPENCLAW_PROJECT_DIR:-$(pwd)}"
INSTANCE=$(basename "$INSTANCE_DIR" | sed 's/^\.openclaw-//')
cd "${OPENCLAW_PROJECT_DIR:-/docker/openclaw-$INSTANCE}"
docker compose exec -u root openclaw-gateway chown -R node:node /home/node/.openclaw/
```
For non-Docker deployments, skip this step.

### 10. Verify
After writing:
- Check file size against limits
- Verify consistency with other workspace files
- Suggest next steps (e.g., "Now optimize your AGENTS.md to match")

## For `all` mode
Process files in this order:
1. IDENTITY.md (foundational)
2. SOUL.md (persona)
3. USER.md (users)
4. AGENTS.md (rules)
5. TOOLS.md (tools)
6. HEARTBEAT.md (background tasks)
7. MEMORY.md (memory structure)
8. openclaw.json (configuration — with user permission)

Ask for approval after each file before proceeding to the next.
Run security check across all files at the end.
Fix permissions once after all edits are complete.

</details>

### `workspace-scan`

Quick health check of OpenClaw instance — scans all categories (A–J), checks sizes, detects inline secrets, identifies missing components, verifies language

Arguments: `[quick|full|security]`

<details><summary>Prompt template</summary>

# Workspace Scan

Perform a quick health check of the current OpenClaw instance. The active instance dir is `$OPENCLAW_PROJECT_DIR` when set, otherwise the current working directory (standard: `~/.openclaw/`, Docker multi-instance: `~/.openclaw-{name}/`).

## Process

### 1. Resolve and verify instance dir
```bash
INSTANCE_DIR="${OPENCLAW_PROJECT_DIR:-$(pwd)}"
cd "$INSTANCE_DIR" || { echo "ERROR: cannot access $INSTANCE_DIR"; exit 1; }
[ -f ./openclaw.json ] && echo "openclaw.json: OK" || echo "WARNING: not in an OpenClaw instance root ($INSTANCE_DIR)"
[ -d ./workspace ] && echo "workspace/: OK" || echo "WARNING: workspace/ not found"
echo "Instance dir: $INSTANCE_DIR"
```

### 2. Scan auto-injected workspace files (Cat A — 7 core files)

Only these 7 files are auto-injected into the agent's context every session:
```bash
for f in AGENTS.md SOUL.md USER.md IDENTITY.md TOOLS.md HEARTBEAT.md MEMORY.md; do
  if [ -f "./workspace/$f" ]; then
    CHARS=$(wc -c < "./workspace/$f")
    WORDS=$(wc -w < "./workspace/$f")
    MOD=$(stat -f "%Sm" -t "%Y-%m-%d" "./workspace/$f" 2>/dev/null || stat -c "%y" "./workspace/$f" 2>/dev/null | cut -d' ' -f1)
    echo "OK  $f  ${CHARS}c  ${WORDS}w  $MOD"
  else
    echo "MISSING  $f"
  fi
done
```

### 2b. Check special lifecycle files (BOOT.md, BOOTSTRAP.md)

BOOTSTRAP.md IS auto-injected when present (on new workspaces, deleted after bootstrap). BOOT.md is NOT auto-injected — it runs via hook on gateway restart only.
```bash
# BOOT.md — optional persistent startup script (executed via hook, not injected)
if [ -f "./workspace/BOOT.md" ]; then
  echo "OK  BOOT.md  $(wc -c < "./workspace/BOOT.md")c  $(wc -w < "./workspace/BOOT.md")w  (optional startup script)"
else
  echo "—   BOOT.md  not configured (optional)"
fi

# BOOTSTRAP.md — one-time first-run ritual (deleted after completion)
if [ -f "./workspace/BOOTSTRAP.md" ]; then
  echo "WARN  BOOTSTRAP.md  PRESENT — bootstrap has not completed yet"
else
  echo "OK  BOOTSTRAP.md  absent (bootstrap completed)"
fi
```

### 2c. Detect extra files in workspace/ root

Agents can create arbitrary files in workspace/ during sessions. These are NOT auto-injected and don't count toward character limits:
```bash
for f in ./workspace/*.md; do
  [ -f "$f" ] || continue
  BASENAME=$(basename "$f")
  case "$BASENAME" in
    AGENTS.md|SOUL.md|USER.md|IDENTITY.md|TOOLS.md|HEARTBEAT.md|MEMORY.md|BOOT.md|BOOTSTRAP.md) continue ;;
    *) echo "EXTRA: $BASENAME ($(wc -c < "$f") chars) — not auto-injected, not counted toward limits" ;;
  esac
done
```
If extra files are found, note: "These files were created by the agent during sessions. They don't consume context tokens. If they contain reference material the agent needs, consider moving them to `workspace/docs/` and adding a reference in AGENTS.md."

### 3. Check character limits (auto-injected files only)

Limits apply to the 7 auto-injected files plus BOOTSTRAP.md while present. BOOT.md and agent-created files are NOT counted:
- Per-file limit: 20,000 chars (default). Flag files exceeding this.
- Total limit: 150,000 chars across all auto-injected files.
```bash
TOTAL=0
for f in AGENTS.md SOUL.md USER.md IDENTITY.md TOOLS.md HEARTBEAT.md MEMORY.md; do
  if [ -f "./workspace/$f" ]; then
    CHARS=$(wc -c < "./workspace/$f")
    TOTAL=$((TOTAL + CHARS))
    [ "$CHARS" -gt 15000 ] && echo "WARNING: $f is ${CHARS}c — approaching 20K truncation limit. Consider moving infrequently-used sections to docs/"
    [ "$CHARS" -gt 20000 ] && echo "CRITICAL: $f is ${CHARS}c — EXCEEDS 20K limit, content will be truncated!"
  fi
done
echo "Auto-injected total: $TOTAL chars (limit: 150,000)"
```

If any auto-injected file is near the limit, recommend: "Use the 50% rule — if content is needed in less than 50% of sessions, move it to `workspace/docs/` and reference from AGENTS.md. See subfolder-patterns for templates."

### 4. Check openclaw.json (Cat E)
```bash
if [ -f ./openclaw.json ]; then
  echo "openclaw.json: $(wc -c < ./openclaw.json) bytes"
else
  echo "openclaw.json: NOT FOUND"
fi
```

### 5. Secret detection in openclaw.json
```bash
# Check for potential inline secrets
grep -n -E '"[A-Za-z0-9_:.-]{20,}"' ./openclaw.json 2>/dev/null | grep -vi '"source"\|"provider"\|"id"\|"model"\|"profile"\|"mode"\|"workspace"\|"description"\|"name"'
```

If matches found, warn user:
- Inline secrets are a security risk
- Recommend SecretRef pattern: `{ "source": "env", "provider": "default", "id": "ENV_VAR_NAME" }`
- `openclaw doctor` SecretRef resolution errors outside gateway runtime are safe to ignore

### 6. Check workspace subfolders (Cat B, C, D, J)
```bash
echo "--- Memory Logs (Cat B) ---"
ls ./workspace/memory/ 2>/dev/null | wc -l

echo "--- Instance Skills (Cat C) ---"
find ./workspace/skills/ -name "SKILL.md" -type f 2>/dev/null | wc -l

echo "--- Docs (Cat D) ---"
find ./workspace/docs/ -name "*.md" -type f 2>/dev/null | wc -l

echo "--- Workflows (Cat D) ---"
find ./workspace/workflows/ -name "*.prose" -type f 2>/dev/null | wc -l

echo "--- Canvas (Cat J) ---"
ls ./workspace/canvas/ 2>/dev/null | wc -l

echo "--- Standing Orders ---"
ls ./workspace/docs/standing-orders/ 2>/dev/null | wc -l
```

### 7. Check sessions (Cat F)
```bash
echo "--- Sessions ---"
ls ./agents/main/sessions/*.jsonl 2>/dev/null | wc -l
[ -f ./agents/main/sessions/sessions.json ] && echo "sessions.json: $(jq length ./agents/main/sessions/sessions.json 2>/dev/null) entries" || echo "sessions.json: MISSING"
```

### 8. Check memory index (Cat G)
```bash
[ -f ./memory/main.sqlite ] && echo "memory index: $(ls -lh ./memory/main.sqlite | awk '{print $5}')" || echo "memory index: NOT FOUND"
```

### 9. Check cron jobs (Cat H)
```bash
if [ -f ./cron/jobs.json ]; then
  echo "cron/jobs.json: $(wc -c < ./cron/jobs.json) bytes"
else
  ls ./cron/ 2>/dev/null || echo "cron/: NOT FOUND"
fi
```

### 10. Check logs (Cat I)
```bash
if [ -f ./logs/openclaw.log ]; then
  echo "openclaw.log: $(wc -l < ./logs/openclaw.log) lines"
  echo "Recent errors: $(grep -ci 'error\|fatal' ./logs/openclaw.log 2>/dev/null)"
else
  echo "openclaw.log: NOT FOUND"
fi
```

### 11. Check shared skills & plugins
```bash
echo "--- Managed Skills (standard path) ---"
find ~/.openclaw/skills/ -name "SKILL.md" -type f 2>/dev/null | wc -l

echo "--- Shared Public Skills (Docker path) ---"
find /root/openclaw-skills/ -name "SKILL.md" -type f 2>/dev/null | wc -l

echo "--- Shared Private Skills (Docker path) ---"
find /root/openclaw-private-skills/ -name "SKILL.md" -type f 2>/dev/null | wc -l

echo "--- Shared Public Plugins (Docker path) ---"
ls /root/openclaw-plugins/packages/ 2>/dev/null | wc -l

echo "--- Shared Private Plugins (Docker path) ---"
ls /root/openclaw-plugins-private/packages/ 2>/dev/null | wc -l
```

### 12. Bootstrap status
Already checked in step 2b. If BOOTSTRAP.md is present, remind the user that bootstrap hasn't completed — the agent should run it or the file should be deleted.

### 13. Quick security flags
- Does AGENTS.md have a "Red Lines" section?
- Does SOUL.md have a "Boundaries" section?
- Is dmPolicy set to "allowlist" (not "open")?

### 14. Output summary table

```
Auto-injected files (7 core — counted toward context limits):
| File          | Status  | Size    | Words | Last Modified | Issues        |
|---------------|---------|---------|-------|---------------|---------------|
| AGENTS.md     | OK      | 3,200c  | 450w  | 2025-03-15    |               |
| SOUL.md       | OK      | 1,800c  | 280w  | 2025-03-10    |               |
| USER.md       | MISSING | -       | -     | -             | Create this!  |
| ...           | ...     | ...     | ...   | ...           | ...           |

Special lifecycle files (NOT auto-injected):
| File           | Status                          |
|----------------|---------------------------------|
| BOOT.md        | Not configured (optional)       |
| BOOTSTRAP.md   | Absent — bootstrap completed OK |

Extra workspace files (NOT auto-injected, not counted toward limits):
| File                             | Size     | Recommendation                    |
|----------------------------------|----------|-----------------------------------|
| research-agent-marketplaces.md   | 231,000c | Move to docs/ or delete           |

Instance: [CWD]
Core files: X/7 present, Y chars total (limit: 150,000)
BOOT.md: [configured/not configured] | BOOTSTRAP.md: [completed/WARNING: present]
Extra workspace files: N (not auto-injected)
Docs: N files | Skills: N | Memory: N daily logs | Workflows: N
Sessions: N JSONL | Cron: [configured/none] | Log errors: N
Memory index: [present/absent] | Canvas: [N files/none]
Shared: N public skills, N private skills, N public plugins, N private plugins
Security: [OK / WARNINGS (details)]
```

### 15. Quick recommendations
List top 3 immediate actions based on scan results.

</details>
