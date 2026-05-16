# dify-ops

> Dify self-hosted update operations plugin. Pull upstream changes, merge into local dev branch, sync .env variables, detect Docker project names, and rebuild containers for Dify Docker deployments.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/dify-ops

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **dify-docker-architecture** — Dify Docker Compose deployment architecture — services, container naming, directory layout, .env.example structure, and Docker project name conventions. Use when working with Dify Docker setup, understanding container services, debugging container issues, or needing to know the Dify directory structure. Triggers on "dify docker", "dify containers", "dify services", "dify architecture", "dify compose".

- **env-sync** — Synchronize .env with .env.example for Dify Docker deployments — detect new variables, add missing ones with default values, preserve existing customizations. Use when syncing env variables, checking for new Dify configuration variables, comparing .env.example vs .env, "env sync", "new env variables", "missing environment variables", or after pulling Dify updates.

- **examples** — End-to-end scenario walkthroughs for updating self-hosted Dify. Use when the user asks for "dify update example", "how to update dify", "show me an update walkthrough", "dify upgrade guide", "step-by-step dify update", or needs a complete example of the update process.

- **update-workflow** — Git workflow for updating self-hosted Dify — fetch upstream, merge main into dev, handle conflicts, checkout specific tags. Use when updating Dify, merging upstream changes, handling merge conflicts in Dify, or switching to a specific Dify version/tag. Triggers on "update dify", "pull dify changes", "merge main into dev", "upgrade dify", "dify version".


## Subagents

Defined under `.codex/agents/` as TOML files:

- **dify-updater** — Dify self-hosted update operations agent. Handles updating Dify Docker deployments — pulling
upstream changes, merging into local dev branch, syncing environment variables, detecting
Docker project names, and rebuilding containers.

<example>
Context: User wants to update to latest version
user: "Update my Dify to the latest version"
assistant: "I'll check your current Dify state and pull the latest changes from upstream."
<commentary>
Standard update request — agent runs the full update workflow: fetch, merge, env sync, rebuild.
</commentary>
</example>

<example>
Context: User wants a specific version
user: "Update Dify to version 0.15.0"
assistant: "I'll fetch tags and merge version 0.15.0 into your dev branch."
<commentary>
Tagged release update — agent verifies tag exists, merges into dev, handles conflicts.
</commentary>
</example>

<example>
Context: User wants to check status
user: "Check if my Dify needs updating"
assistant: "I'll check your current version against upstream and report the status."
<commentary>
Read-only check — agent compares local commit with origin/main and reports.
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
assistant: "I'll compare your .env against .env.example and add any missing variables."
<commentary>
Standalone env sync — agent diffs the two files and appends missing variables.
</commentary>
</example>


## Workflows (canonical slash commands)

Codex CLI doesn't support custom slash commands — invoke these workflows via natural language. Each entry below is a prompt template you can adapt:

### `status`

Show current Dify instance status — git branch, version, container health, .env sync state

<details><summary>Prompt template</summary>

# Dify Status

Show current state of a self-hosted Dify instance without making any changes.

## Process

### Step 1: Detect Working Directory

```bash
if [ -f "docker-compose.yaml" ] && [ -f ".env.example" ]; then
  DOCKER_DIR="$(pwd)"
  DIFY_ROOT="$(dirname $(pwd))"
elif [ -d "docker" ] && [ -f "docker/docker-compose.yaml" ]; then
  DOCKER_DIR="$(pwd)/docker"
  DIFY_ROOT="$(pwd)"
else
  echo "ERROR: Cannot find Dify Docker setup."
  echo "Run this from dify/ or dify/docker/ directory."
fi
```

### Step 2: Git Status

```bash
cd "$DIFY_ROOT"
echo "=== Git Status ==="
echo "Branch: $(git branch --show-current)"
echo "Commit: $(git rev-parse --short HEAD)"
echo "Date:   $(git log -1 --format=%ci)"

# Check for uncommitted changes
DIRTY=$(git status --porcelain)
[ -z "$DIRTY" ] && echo "Working tree: clean" || echo "Working tree: dirty ($(echo "$DIRTY" | wc -l | tr -d ' ') files)"

# Check how far behind upstream
git fetch origin --quiet 2>/dev/null
BEHIND=$(git rev-list --count HEAD..origin/main 2>/dev/null)
[ "$BEHIND" -gt 0 ] && echo "Behind origin/main: $BEHIND commits" || echo "Up to date with origin/main"

# Latest tag info
LATEST_TAG=$(git tag --sort=-creatordate 2>/dev/null | head -1)
CURRENT_TAG=$(git describe --tags --abbrev=0 2>/dev/null || echo "none")
echo "Current tag: $CURRENT_TAG"
echo "Latest tag:  $LATEST_TAG"
```

### Step 3: .env Sync Check

```bash
cd "$DOCKER_DIR"
echo ""
echo "=== Environment Status ==="

if [ ! -f ".env" ]; then
  echo ".env: MISSING — run /dify-ops:update to create"
else
  grep -E '^[A-Z_][A-Z0-9_]*=' .env.example | cut -d= -f1 | sort > /tmp/dify-example-keys
  grep -E '^[A-Z_][A-Z0-9_]*=' .env | cut -d= -f1 | sort > /tmp/dify-env-keys
  NEW_COUNT=$(comm -23 /tmp/dify-example-keys /tmp/dify-env-keys | wc -l | tr -d ' ')
  REMOVED_COUNT=$(comm -13 /tmp/dify-example-keys /tmp/dify-env-keys | wc -l | tr -d ' ')

  echo ".env: exists"
  [ "$NEW_COUNT" -gt 0 ] && echo "Missing vars: $NEW_COUNT (run /dify-ops:update to add)" || echo "Missing vars: 0 — in sync"
  [ "$REMOVED_COUNT" -gt 0 ] && echo "Extra vars:   $REMOVED_COUNT (may be deprecated or custom)"

  rm -f /tmp/dify-example-keys /tmp/dify-env-keys
fi
```

### Step 4: Docker Container Status

```bash
echo ""
echo "=== Container Status ==="

CONTAINER=$(docker ps --format '{{.Names}}' 2>/dev/null | grep -E '-(api|web|worker)-' | head -1)
if [ -n "$CONTAINER" ]; then
  PROJECT_NAME=$(echo "$CONTAINER" | sed 's/-\(api\|web\|worker\|nginx\|redis\|db_postgres\|sandbox\|ssrf_proxy\|plugin_daemon\|weaviate\|worker_beat\)-[0-9]*$//')
  echo "Docker project: $PROJECT_NAME"
  cd "$DOCKER_DIR"
  docker compose ps 2>/dev/null || docker ps --filter "name=$PROJECT_NAME" --format "table {{.Names}}\t{{.Status}}"
else
  echo "No running Dify containers detected"
fi
```

### Step 5: Health Check

```bash
NGINX_PORT=$(grep -E '^(EXPOSE_NGINX_PORT|NGINX_PORT)=' "$DOCKER_DIR/.env" 2>/dev/null | tail -1 | cut -d= -f2)
NGINX_PORT=${NGINX_PORT:-80}
echo ""
echo "=== Health ==="
curl -s -o /dev/null -w "Web UI (port $NGINX_PORT): HTTP %{http_code}" "http://localhost:${NGINX_PORT}" 2>/dev/null || echo "Web UI: not reachable"
```

</details>

### `update`

Update self-hosted Dify — pull upstream, merge into dev, sync .env, rebuild Docker containers

Arguments: `[tag-or-version]`

<details><summary>Prompt template</summary>

# Dify Update

Update a self-hosted Dify instance: pull upstream changes, merge into local dev branch, sync environment variables, and rebuild Docker containers.

## Arguments

- `$ARGUMENTS` — optional tag/version (e.g., `0.15.0`, `v0.15.0`). If empty, update to latest main.

## Process

### Step 1: Detect Working Directory

Determine whether user is in `dify/` root or `dify/docker/`.

```bash
if [ -f "docker-compose.yaml" ] && [ -f ".env.example" ]; then
  DOCKER_DIR="$(pwd)"
  DIFY_ROOT="$(dirname $(pwd))"
elif [ -d "docker" ] && [ -f "docker/docker-compose.yaml" ]; then
  DOCKER_DIR="$(pwd)/docker"
  DIFY_ROOT="$(pwd)"
else
  echo "ERROR: Cannot find Dify Docker setup."
  echo "Run this from dify/ or dify/docker/ directory."
  exit 1
fi
echo "DIFY_ROOT=$DIFY_ROOT"
echo "DOCKER_DIR=$DOCKER_DIR"
```

### Step 2: Pre-flight Checks

Run from DIFY_ROOT:

```bash
cd "$DIFY_ROOT"
git rev-parse --git-dir 2>/dev/null && echo "Git repo: OK" || { echo "ERROR: Not a git repo"; exit 1; }
echo "Branch: $(git branch --show-current)"
PRE_UPDATE_COMMIT=$(git rev-parse --short HEAD)
echo "Commit: $PRE_UPDATE_COMMIT"
DIRTY=$(git status --porcelain)
if [ -n "$DIRTY" ]; then
  echo "WARNING: Uncommitted changes:"
  git status --short
fi
```

If dirty working tree, ask user:
1. Commit changes first
2. Stash changes (`git stash`)
3. Abort update

If stashing, record `STASHED=true`.

### Step 3: Fetch Upstream

```bash
cd "$DIFY_ROOT"
git fetch origin --tags
echo "Fetched latest from origin"
```

### Step 4: Merge

**If no tag specified (update to latest main):**

```bash
cd "$DIFY_ROOT"
CURRENT=$(git branch --show-current)
[ "$CURRENT" != "dev" ] && git checkout dev
git merge origin/main
```

**If tag specified:**

```bash
cd "$DIFY_ROOT"
TAG="$ARGUMENTS"

# Verify tag exists (try with and without 'v' prefix)
if ! git tag -l "$TAG" | grep -q .; then
  if git tag -l "v$TAG" | grep -q .; then
    TAG="v$TAG"
  else
    echo "ERROR: Tag '$TAG' not found. Recent tags:"
    git tag --sort=-creatordate | head -10
    # STOP — ask user to select a valid tag
  fi
fi

git checkout dev
git merge "$TAG"
```

**If merge conflicts occur:**

1. List conflicted files: `git diff --name-only --diff-filter=U`
2. For `.env.example`: accept theirs (`git checkout --theirs docker/.env.example && git add docker/.env.example`)
3. For `docker-compose.yaml`: show conflicts, help user merge manually (preserve their custom ports/volumes, accept upstream service changes)
4. For other files: show diff, let user decide
5. After resolving: `git add . && git commit -m "Merge upstream into dev"`

### Step 5: Sync .env

```bash
cd "$DOCKER_DIR"

if [ ! -f ".env" ]; then
  cp .env.example .env
  echo "Created .env from .env.example — review security-sensitive values"
else
  # Check for official sync script
  if [ -f "dify-env-sync.sh" ]; then
    echo "Found official dify-env-sync.sh — offer to use it"
  fi

  # Find new variables
  grep -E '^[A-Z_][A-Z0-9_]*=' .env.example | cut -d= -f1 | sort > /tmp/dify-example-keys
  grep -E '^[A-Z_][A-Z0-9_]*=' .env | cut -d= -f1 | sort > /tmp/dify-env-keys
  NEW_KEYS=$(comm -23 /tmp/dify-example-keys /tmp/dify-env-keys)

  if [ -z "$NEW_KEYS" ]; then
    echo "No new environment variables. .env is up to date."
  else
    echo "New variables found — show table with defaults and security flags"
    echo "Ask user to confirm before appending"
    # Append with date header comment
  fi

  rm -f /tmp/dify-example-keys /tmp/dify-env-keys
fi
```

Show new variables as a table. Flag security-sensitive ones (`*SECRET*`, `*PASSWORD*`, `*KEY*`, `*TOKEN*`). Ask for confirmation before appending.

### Step 6: Detect Docker Project Name

```bash
CONTAINER=$(docker ps --format '{{.Names}}' 2>/dev/null | grep -E '-(api|web|worker)-' | head -1)

if [ -n "$CONTAINER" ]; then
  PROJECT_NAME=$(echo "$CONTAINER" | sed 's/-\(api\|web\|worker\|nginx\|redis\|db_postgres\|sandbox\|ssrf_proxy\|plugin_daemon\|weaviate\|worker_beat\)-[0-9]*$//')
  echo "Detected Docker project: $PROJECT_NAME"
else
  echo "No running Dify containers found. Will use default compose behavior."
  PROJECT_NAME=""
fi
```

### Step 7: Rebuild Containers

```bash
cd "$DOCKER_DIR"

if [ -n "$PROJECT_NAME" ] && [ "$PROJECT_NAME" != "docker" ]; then
  echo "Running: docker compose -p $PROJECT_NAME up -d --build"
  docker compose -p "$PROJECT_NAME" up -d --build
else
  echo "Running: docker compose up -d --build"
  docker compose up -d --build
fi
```

### Step 8: Verify

```bash
cd "$DOCKER_DIR"
sleep 15
docker compose ps 2>/dev/null || docker ps --filter "name=${PROJECT_NAME:-docker}"

# Check web reachability
NGINX_PORT=$(grep -E '^(EXPOSE_NGINX_PORT|NGINX_PORT)=' "$DOCKER_DIR/.env" 2>/dev/null | tail -1 | cut -d= -f2)
NGINX_PORT=${NGINX_PORT:-80}
curl -s -o /dev/null -w "HTTP %{http_code}" "http://localhost:${NGINX_PORT}" 2>/dev/null && echo " — Dify reachable" || echo " — Not reachable yet (may still be starting)"
```

### Step 9: Summary

Print update summary:
- Previous commit → current commit
- Branch and merge source
- New env variables added (count + any requiring attention)
- Conflicts resolved (if any)
- Docker project name used
- Container status
- Rollback command with warning about DB migrations

If stashed earlier: `git stash pop` and show result.

</details>
