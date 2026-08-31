# openclaw-ops

> Operations plugin for a fleet of self-hosted OpenClaw gateway instances running as Docker Compose projects on one host. Discovers every instance from the live Docker state (never from hard-coded paths), classifies it ok/degraded/down/alien, and runs day-two maintenance: health and liveness reporting, provider-auth triage (expired, emptied and shadowed OAuth profiles, shared-credential token sink), config surgery with snapshot and executable rollback, memory/embedding repair and reindexing, shared skills and plugins consolidation, Infisical secret-delivery audit by key name only, security audit, version-drift and channel-aware upgrades, and reference-instance cloning. Mutations are dry-run by default behind an eight-block plan, need --yes, and need a typed confirmation when irreversible. Secrets are reported as fingerprints, presence and expiry — never as values. File-based knowledge: no MCP server, no required environment variables, no stored credentials; the single optional variable OPENCLAW_OPS_CONFIG is an escape hatch for the fleet-config path, and deployment specifics live in that operator-owned config outside the repository.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/openclaw-ops

## Skills

- **config-surgery** — Use when an OpenClaw instance config is about to be read, changed, restored, split or explained — a model chain, channel, tool, plugin, skill, session or memory setting, a secret reference, an include, a gateway that refuses to start after an edit, a config the process appears to ignore, a change that looks applied and has no effect, an edit that removes lines, a stray backup or rejected sidecar file next to the config, or the question of what needs a restart.
- **docs-research** — Use when anything about OpenClaw is about to be stated or recommended that could have changed — a config key, a CLI flag or subcommand spelling, an auth method, a release channel, a current version, a model name, whether a feature exists or is deprecated — and whenever a claim needs a citation, two sources disagree, an instance's own documentation looks older than the project's, or the situation is offline and it must be decided what may still be said without a live source.
- **examples** — Use when a whole OpenClaw fleet job is in view rather than a single answer — first contact with a host whose instances are unknown, a fleet where every instance lost its provider login, an upgrade whose rollback story is unclear, a new instance that has to be stood up and proven isolated — and whenever the question is what the entire sequence looks like, which command owns which step, what a run of this plugin produces end to end, or where one step's output becomes the next step's input.
- **fleet-diagnostics** — Use when an OpenClaw instance is misbehaving or suspected of it — health green but nothing is happening, a container restarting or unhealthy, an empty or ignored config, models returning unauthorized or reporting logged out, tokens that are present but empty, schedules failing or firing twice or stalling, memory search dead or paused or a state database growing without limit, keys missing inside the container, shared skills that look installed and do nothing, a version that does not match its siblings, or a port published where it should not be. Also use when a report, audit or repair needs a stable finding id.
- **fleet-model** — Use when work touches an OpenClaw gateway instance or several of them on one host — inventory, status, health, logs, provider auth, config, secrets, memory, shared skills, upgrades, cloning — or when an instance is named or selected, or when the alternative would be a hand-written docker exec, a guessed path, or a remembered version or model name.
- **instance-clone** — Use when a new OpenClaw instance is to be created from an existing one — cloning the reference, standing up a canary or a throwaway test instance, adding an instance for a new workload or tenant, picking a free gateway port for one, deciding what a new instance may share with its source and what it must not — and also when an instance created earlier behaves like its source, answers on the wrong port, has no credentials, or is suspected of not being isolated from the instance it was copied from.
- **instance-upgrade** — Use when an OpenClaw instance or a fleet of them is being upgraded, or when the question is which version to move to — version drift between instances, what the current stable is, a release channel or a registry dist-tag, an image tag or digest pin, a soak or hold-back window, a gateway that will not start after an update, schedules that started firing several times per tick, a session that expired some time after an update, or what a given release added.
- **memory-ops** — Use when OpenClaw memory or its embeddings are involved — embedding calls failing authorization or reporting an invalid token, vector search paused or returning nothing useful, an index-identity warning, a last-index timestamp far in the past, search still poor after the provider was fixed, choosing or changing an embedding provider, model or key, a reindex, or a state database that keeps growing and needs retention or compaction. This skill owns the subscription-session-versus-embedding-key distinction; chat-side credential and OAuth problems are provider-auth.
- **provider-auth** — Use when model-provider credentials are in question — unauthorized responses, an instance reporting itself logged out, tokens present but empty, an expiry approaching or passed, several instances losing the same account at once, a login that has to be performed, a choice between an API key, provider OAuth and a local CLI backend, billing that moved to metered tokens without a config change, a provider CLI reporting not-logged-in inside its container, or credential directories being shared, split or copied between instances. Embedding and vector-search failures are memory-ops, not this skill.
- **secrets-infisical** — Use when secrets reach an OpenClaw instance through an injection wrapper and something about that is in question — a feature silently off while the config looks right, an instance receiving far fewer keys than its siblings, a plaintext env file inside the state tree, a token or client id sitting in a backup or identity copy, a key that has to be added, rotated, or proven delivered, a machine identity or its project binding, a secret reference in the config, or a restart whose safety depends on every referenced key still resolving.
- **security-audit** — Use when the security posture of an OpenClaw instance or of the whole fleet is in question — a gateway port that may be reachable from outside the host, firewall rules that read correctly but may not apply to published ports, an operator bearer token that may be shared between instances or sitting in a plaintext file, permissions on state, credential or identity trees, a metrics or admin endpoint answering without authentication, a secret found in a backup or an identity copy, a question about who can reach an instance and what that access grants, before exposing an instance to a new network or new people, and after any suspected compromise.
- **shared-assets** — Use when skills or plugins are shared across OpenClaw instances on one host — shared trees mounted but empty, the same skill copied into every instance, a shared copy edited with no change in behaviour, an installed asset that never appears in the registered list, a plugin change that did nothing, a load candidate refused over ownership, a suspect install lock, or any request to deduplicate, promote, register, verify or roll out shared skills and plugins.
- **audit** — Full fleet audit in an isolated agent — one prioritised report with a ready repair line under every finding
- **auth** — Provider credential state across the fleet — classify every profile, print the logins a human must run, watch for expiry and account drift
- **clone** — Create a new instance from the reference one — isolation preflight, a genuinely free port, materialisation of compose and state, then the remaining manual steps printed rather than skipped
- **exec** — Run one openclaw CLI call against selected instances through the single audited door
- **features** — Read the release notes across a version range and report which newly added config keys and commands this fleet could adopt — per instance, with a recommendation, a risk class and a reason. Nothing is switched on.
- **init** — First-run wizard — preflight this host, detect the OpenClaw instances on it, write the operator-owned fleet config
- **logs** — Fan out gateway logs across selected instances, redacted, with per-instance headers
- **repair** — The only repair funnel — dispatch one catalog finding id through its eight-block plan, apply on a later turn, verify
- **shared-sync** — Deduplicate skills and plugins across instances, promote the identical ones into the shared trees, register them, remove the local copies that shadow them, and verify on four levels
- **status** — Daily fleet picture — one row per instance, with HEALTH and LIVENESS as two independent verdicts
- **update** — Upgrade instances on a pinned, soaked target — baseline, verified backup, plan, apply, post-checks, schedule dedup

## Subagents

Codex does not install plugin subagents automatically — copy them manually before use:

```bash
cp agents/*.toml ~/.codex/agents/        # personal
cp agents/*.toml <repo>/.codex/agents/    # project-local
```

- **openclaw-fleet-auditor** — Use this agent for a full read-only sweep of an OpenClaw fleet across every axis — inventory and layout, liveness, config, provider auth, model chain, memory and embeddings, schedules, secret delivery, shared assets, version drift, security exposure — cross-checked against the findings catalog, the upstream-issues catalog and the live documentation, returning one prioritised report with a ready-to-run repair line under each finding.

<example>
Context: The operator has not looked at the fleet in weeks and wants to know what is wrong before touching anything.
user: "Audit the whole fleet and tell me what needs attention."
assistant: "I'll use the openclaw-fleet-auditor agent to sweep every axis read-only and come back with one prioritised report."
<commentary>The whole-fleet sweep is the agent's core flow: many cheap reads whose intermediate output is noise in the conversation, one document as the result.</commentary>
</example>

<example>
Context: Every instance reports healthy, yet scheduled jobs produce nothing.
user: "All the gateways say they're healthy but nothing is actually getting done. Find out why."
assistant: "I'll use the openclaw-fleet-auditor agent — health and liveness are computed from disjoint evidence, and a green instance whose log has not moved is exactly what the sweep separates."
<commentary>Self-reported health versus observed activity: the disagreement is the finding, and it only appears when both are gathered fleet-wide.</commentary>
</example>

<example>
Context: Before an upgrade wave, the operator wants the pre-existing findings recorded.
user: "We're upgrading next week. What's already broken, so we can tell new breakage from old?"
assistant: "I'll use the openclaw-fleet-auditor agent to capture the baseline — the pre-upgrade finding set is what makes a post-upgrade regression identifiable."
<commentary>Baseline capture: only new findings may block an upgrade, so the baseline has to exist before it.</commentary>
</example>

<example>
Context: The operator asks for a fix while the audit is running.
user: "Just fix the expired logins while you're in there."
assistant: "The auditor is read-only by construction. I'll finish the sweep, then hand you the repair line for that finding to run through /openclaw-ops:repair."
<commentary>The agent never mutates; the --yes gate lives in the command layer, where a human can answer it.</commentary>
</example>

- **openclaw-incident-responder** — Use this agent to diagnose ONE broken OpenClaw instance in depth — a container in a crash loop, a gateway that is green but has stopped doing work, a search that stays degraded after its provider was fixed, a schedule that stalls for hours or stopped firing altogether, a gateway left stopped after an upgrade — reading logs iteratively, testing hypotheses against evidence, and localising the cause to a single finding id with one targeted action for a human to run.

<example>
Context: One container keeps restarting.
user: "The gateway for that instance is restarting over and over. Fix it."
assistant: "I'll use the openclaw-incident-responder agent to read the first failed start rather than the latest one — restarting a crash loop overwrites the log line that holds the cause."
<commentary>Crash loop: zero restart attempts before the cause is known. The agent diagnoses and hands back one targeted action.</commentary>
</example>

<example>
Context: An instance passes every probe and produces nothing.
user: "This one says it's healthy, but its log hasn't moved in weeks and no job has run."
assistant: "I'll use the openclaw-incident-responder agent — health green with a stale log is the zombie signature, and it is a symptom of an auth, secret or config failure underneath."
<commentary>The zombie: every self-report comes from the process that stopped working, so the diagnosis has to come from outside it.</commentary>
</example>

<example>
Context: Memory search is still bad after the embedding key was replaced.
user: "We fixed the embedding key days ago and search is still returning nothing useful."
assistant: "I'll use the openclaw-incident-responder agent to check whether search is stuck on the fallback model and whether the index identity changed with the key."
<commentary>Two distinct causes with one symptom; the agent separates them before anything is restarted or reindexed.</commentary>
</example>

<example>
Context: The user wants the responder to apply the fix.
user: "You found it — go ahead and restart it and reindex."
assistant: "The responder is read-only. I'll hand you the finding id and the single action to run through the repair command, where the plan and the --yes gate live."
<commentary>Diagnosis and mutation are separated on purpose: the agent that formed the hypothesis is the worst judge of whether to act on it unsupervised.</commentary>
</example>


## Hooks

This plugin ships `hooks/hooks.json`. After installing, run `/hooks` to trust it before its hooks will run. Note: `PreToolUse.additionalContext` is currently ignored by Codex.
