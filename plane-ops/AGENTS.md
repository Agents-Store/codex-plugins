# plane-ops

> Plane Agile Ops knowledge plugin. Full coverage of the Plane MCP surface: sprint planning, task decomposition, estimation, backlog management, velocity tracking, retrospectives, standups, intake triage, modules, epics, initiatives, milestones, roadmaps, dependencies, burndown, pages (sprint reports, retros, ADRs, runbooks, specs, meeting notes), labels, workflow states, work item types, custom properties, comments, links, work logs, relations, history, bulk edits, search, members, and assignment. Tool- and instance-agnostic: works with any Plane MCP server or connector via a bootstrap skill that discovers tools across naming conventions.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/plane-ops

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **agile-fundamentals** — Single source of truth for Agile formulas, Definition of Ready, Definition of Done, MoSCoW priority mapping, WSJF scoring, capacity and velocity formulas, WIP limits, Fibonacci story points, and sprint buffer policy. Use whenever the user asks about story points, estimation, velocity, capacity, focus factor, WSJF, DoR/DoD, MoSCoW, Fibonacci, sprint buffer, or any Agile rule — and whenever any other plane-ops skill, command, or agent needs canonical Agile definitions. Do not duplicate these rules elsewhere — link here instead.
- **backlog-management** — Backlog management — MoSCoW prioritization, WSJF scoring, value-effort analysis, backlog health monitoring, grooming workflows. Use when prioritizing backlog, grooming items, or assessing backlog health.
- **connector-bootstrap** — MUST be consulted at the start of ANY Plane-related request before answering the user or declaring tools unavailable. Discovers Plane tools across any MCP server, connector, or instance naming convention (cowork mode, remote MCP, local MCP, self-hosted, cloud). Use when the user mentions sprint, backlog, work item, cycle, epic, module, milestone, initiative, project, issue, ticket, task, standup, retro, estimate, roadmap, board, Plane, or any work-management operation — even if Plane tools are not visible yet. Also use before saying "I don't have access to Plane tools" or "tool not available".
- **daily-standup** — Daily standup support — progress summary, blocker identification, team status updates, sprint progress tracking, async standup. Use when running daily standups, checking team progress, identifying blockers, or generating async standup reports.
- **epics-initiatives-milestones** — Long-horizon planning in Plane — epics, initiatives, and milestones. Use when the user wants to create an epic, group epics under an initiative, track a release milestone, build a roadmap, or report progress on a long-running goal that spans multiple sprints or modules. Clarifies the difference between the three and when to use which.
- **estimation** — Estimation — story points, Fibonacci scale, t-shirt sizing, relative estimation, planning poker facilitation. Use when estimating work items or running estimation sessions.
- **examples** — Tool call patterns, end-to-end workflow examples, and scenario references for Plane Agile workflows. Use when needing reference implementations, complete examples, or tool call patterns.
- **intake-triage** — Plane intake inbox — triage incoming requests, bug reports, and ideas before they enter the backlog. Use when the user wants to review the intake queue, accept or reject incoming items, convert intake items into work items, or set up a triage process. Covers daily/weekly triage rituals and routing rules.
- **labels-states-properties** — Design taxonomies for Plane projects — labels, workflow states, work item types, and custom properties. Use when the user asks how to organize labels, design a state machine, set up custom fields, configure work item types, decide between label/property/type, or audit a noisy taxonomy. Covers naming conventions, recommended sets, and the difference between the four metadata mechanisms.
- **modules** — Plane modules — feature and workstream grouping that cuts across sprints. Use when the user wants to group related work items by feature area, track progress of a larger feature over multiple sprints, create or manage modules, or ask about workstream progress. Modules complement cycles (time-boxed) by organizing work around outcomes (scope-boxed).
- **pages-publishing** — Create and publish Plane pages — sprint reports, retro notes, release notes, roadmap, meeting notes, decision logs (ADRs), specs, runbooks, and any general-purpose page as formatted HTML. Use when the user wants to publish a report, write a sprint summary, create a retro page, generate release notes, share a roadmap, document a decision, write a spec, capture meeting notes, or create any Plane page. Includes reusable HTML templates and Plane editor compatibility rules.
- **project-setup** — Agile-ready project setup — states, labels, work item types, properties, and feature configuration for Agile workflows. Use when setting up a new project or configuring an existing project for Agile.
- **sprint-planning** — Sprint planning — capacity calculation, sprint goal setting, work item selection, cycle creation. Use when planning a new sprint, calculating capacity, or selecting items for a sprint.
- **sprint-review-retro** — Sprint review and retrospective — completion metrics, previous retro action review, Start-Stop-Continue retro, DAKI format, 4Ls format, action item tracking, sprint close. Use when running sprint review, retrospective, closing a sprint, or reviewing previous retro action item status.
- **task-decomposition** — Task decomposition — INVEST criteria, vertical slicing, epic-to-subtask breakdown, story splitting patterns. Use when breaking down epics, stories, or large work items into smaller pieces.
- **velocity-metrics** — Velocity and metrics — historical velocity calculation, sprint burndown analysis, WIP limits, throughput tracking, time effort analysis. Use when calculating velocity, analyzing sprint progress, reviewing team metrics, comparing estimated vs actual effort, or checking time logged per story point.
- **work-items** — Work item operations in Plane — create, read, update, delete, search, assign, label, link, comment, relate, and log time on work items (issues/tasks/tickets). Use when the user wants to create a task, update an issue, add a comment, attach a label, link a PR, set a blocker, log work time, or inspect a specific item. Covers work item types, custom properties, and multi-item batch edits.

## Subagents

Defined under `.codex/agents/` as TOML files:

- **plane-agile-coach** — Agile Coach for Plane project management. Guides startups through sprint planning, backlog management, task decomposition, estimation, retrospectives, standups, velocity tracking, intake triage, modules, epics, milestones, roadmaps, dependencies, and publishing reports as Plane pages.

<example>
user: "Help me plan our next sprint"
</example>
<example>
user: "Break down this epic into user stories"
</example>
<example>
user: "Run a retrospective for the last sprint"
</example>
<example>
user: "Show me our backlog health"
</example>
<example>
user: "Set up a new Agile project in Plane"
</example>
<example>
user: "Triage the intake queue"
</example>
<example>
user: "Show me the roadmap for this quarter"
</example>

- **plane-sprint-planner** — Specialized sprint planner for Plane. Handles capacity calculation, sprint goal setting, work item selection, and sprint creation with Agile best practices for startup teams. Works with any Plane MCP server, connector, or cowork instance.

<example>
user: "Create a 1-week sprint starting Monday with the top priority backlog items"
</example>
<example>
user: "Calculate our team's capacity for the next sprint"
</example>
<example>
user: "What should we include in the next sprint based on our velocity?"
</example>


## Workflows (canonical slash commands)

Codex CLI doesn't support custom slash commands — invoke these workflows via natural language. Each entry below is a prompt template you can adapt:

### `assign`

Assign or unassign a Plane work item to one or more users

Arguments: `<project> <item> <assignee> [more...]`

<details><summary>Prompt template</summary>

# Assign

Assign a work item in Plane to one or more users. Replaces or appends depending on flags.

## Arguments

Format: `<project> <item> <assignee> [more...]`

- `project`: project name or identifier
- `item`: work item identifier (`PROJ-42`) or UUID
- `assignee`: username, email, display name, or `me`
- Additional assignees space-separated for multi-assign
- Flags: `--replace` (default: append), `--unassign <user>`, `--clear` (remove all)

Parse from `"$ARGUMENTS"`.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `update_work_item`, `get_workspace_members`, `get_project_members`, `get_me`.
2. **Resolve project** → `project_id`. **Resolve work item** → `work_item_id` (and current `assignee_ids`).
3. **Resolve users**:
   - `me` → `get_me` → user_id
   - else → `get_project_members({ project_id })` and match by `display_name`, `email`, or `username` (case-insensitive). If no project member matches, fall back to `get_workspace_members`. If still ambiguous, ask the user to choose.
4. **Compute new `assignee_ids`**:
   - default → union(current, new)
   - `--replace` → just the new set
   - `--unassign <user>` → current minus that user
   - `--clear` → empty array
5. **Update** — `update_work_item({ project_id, work_item_id, assignee_ids })`. Note: some Plane builds use `assignees` instead — check schema.
6. **Confirm** — print final assignee list and item identifier.

## Examples

```
/assign "TaskFlow" PROJ-148 alice
/assign "TaskFlow" PROJ-148 me
/assign "TaskFlow" PROJ-148 alice bob --replace
/assign "TaskFlow" PROJ-148 --unassign bob
/assign "TaskFlow" PROJ-148 --clear
```

## Best Practices

- Prefer **single assignee** for accountability. Multi-assign blurs ownership — use it only for pairing or review handoff.
- Don't assign before the item meets Definition of Ready (see `agile-fundamentals` skill).
- When unassigning yourself, leave a comment with handoff context (`/comment add ...`).

</details>

### `backlog-health`

Analyze backlog health — unestimated items, stale items, missing details

Arguments: `<project>`

<details><summary>Prompt template</summary>

# Backlog Health

Generate a health report for the project backlog — identifying gaps and recommending actions.

## Arguments
Format: `<project>`
- project: Project name or identifier (required)

Parse from "$ARGUMENTS".

## Process

0. **Bootstrap connector** — consult the `connector-bootstrap` skill. Probe `ToolSearch` for the action names referenced below (`list_projects`, `list_cycles`, etc.). Match tools by action suffix — never assume a specific MCP prefix. If multiple Plane instances are connected, ask the user which one to use. All formulas and rules come from the `agile-fundamentals` skill.

1. **Resolve project:**
   ```
   list_projects()
   ```

2. **Get all backlog items:**
   ```
   list_work_items({ project_id, per_page: 100 })
   ```
   Filter: items in "backlog" or "unstarted" state groups.

3. **Calculate health metrics:**
   - Total items in backlog
   - Unestimated (point is null)
   - No priority set (priority is "none" or null)
   - No assignee
   - No description (description_html is empty)
   - Oversized (point > 8)
   - Ready items (has estimate + priority + description)

4. **Calculate health score:**
   ```
   health_score = (ready_items / total_items) × 100
   ```

5. **Display health report:**
   ```
   ┌─────────────────────────────────────┐
   │ BACKLOG HEALTH REPORT               │
   ├─────────────────────────────────────┤
   │ Total items:      XX                │
   │ Ready for sprint: XX (XX%)          │
   │ Unestimated:      XX (XX%)          │
   │ No priority:      XX (XX%)          │
   │ Oversized (>8pt): XX (XX%)          │
   │ No description:   XX (XX%)          │
   ├─────────────────────────────────────┤
   │ Health Score: XX% — STATUS          │
   └─────────────────────────────────────┘
   ```

6. **Provide actionable recommendations:**
   Based on the biggest gaps, suggest:
   - /estimate for unestimated items
   - /decompose for oversized items
   - /wsjf-prioritize for unprioritized items

## Example Usage
```
/backlog-health "TaskFlow"
/backlog-health "My Project"
```

</details>

### `bulk-update`

Apply the same change to many Plane work items at once

Arguments: `<project> <items...> <changes...>`

<details><summary>Prompt template</summary>

# Bulk Update

Apply identical field changes to a list of work items. Plane has no native bulk endpoint — this command loops `update_work_item` and reports per-item success/failure.

## Arguments

Format: `<project> <items...> <changes...>`

- `project`: project name or identifier
- `items`: space-separated identifiers (`PROJ-148 PROJ-149 PROJ-150`), or `--from-search "query"`, or `--from-cycle "Sprint 14"`, or `--from-state "In Review"`
- Changes (any combination):
  - `--state <name>`
  - `--priority <p0|p1|p2|...>`
  - `--assignee <user>` (replaces) or `--add-assignee <user>` (appends)
  - `--label add:<name>` / `--label remove:<name>`
  - `--cycle <name>` (move to cycle) or `--cycle none` (remove from cycle)
  - `--module <name>` / `--module remove:<name>`
  - `--milestone <name>`

Parse from `"$ARGUMENTS"`.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `update_work_item`, `list_work_items`, `search_work_items`, plus `add_work_items_to_cycle` / `add_work_items_to_module` / `add_work_items_to_milestone` for the cycle/module/milestone moves.
2. **Resolve project** → `project_id`.
3. **Resolve item set**:
   - explicit list → resolve each identifier to UUID
   - `--from-search` → `search_work_items`
   - `--from-cycle` / `--from-state` → `list_work_items` with filter
   Print the resolved set with title preview and **ask for confirmation** before mutating.
4. **Resolve targets** — state name → state_id; label name → label_id; assignee → user_id; cycle/module/milestone name → IDs.
5. **Apply** — loop items:
   - For field changes (state/priority/assignee/labels) → `update_work_item`. Per-item, fetch current `label_ids`/`assignee_ids` first when adding/removing rather than replacing.
   - For cycle/module/milestone moves → use the corresponding `add_work_items_to_*` bulk tool with the full list (single API call).
6. **Report** — table: `ID | Field | Old → New | Result`. Sum success/failure counts.

## Safety

- **Always show the resolved set and ask "apply to N items? (y/n)"** before any write.
- Refuse to bulk-update >50 items in a single call without `--force`.
- Bulk updates do NOT trigger Plane notifications the same way single updates do — warn the user.
- Operations are NOT atomic. Partial failure is possible. Print failed IDs at the end.

## Examples

```
/bulk-update "TaskFlow" PROJ-148 PROJ-149 PROJ-150 --state "Done"
/bulk-update "TaskFlow" --from-state "In Review" --priority p1
/bulk-update "TaskFlow" --from-cycle "Sprint 14" --label add:carryover --cycle "Sprint 15"
/bulk-update "TaskFlow" --from-search "login bug" --assignee alice
/bulk-update "TaskFlow" PROJ-160 PROJ-161 --milestone "v2.0 Public Beta"
```

## Best Practices

- Run `/find` or `/my-work` first to verify the selection set, then pipe the IDs into `/bulk-update`.
- For sprint carryover, the canonical pattern is: `/cycles transfer` (handles incomplete-only) — only use bulk-update for cross-cutting field edits.
- After bulk-update, run `/history` on a sample to verify the change landed.

</details>

### `burndown`

Show sprint burndown and projected completion for the active cycle

Arguments: `<project> [--cycle <current|name>]`

<details><summary>Prompt template</summary>

# Burndown

Compute a burndown view for a sprint: ideal line, actual points remaining, and projected end state.

## Arguments

Format: `<project> [--cycle <current|name>]`

Parse from `"$ARGUMENTS"`. Default: current cycle.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `list_cycles`, `list_cycle_work_items`, `list_work_item_activities`.
2. **Resolve project and cycle** — `list_projects` → `list_cycles({ project_id })` → active cycle (or named).
3. **Load cycle items** — `list_cycle_work_items({ project_id, cycle_id })`.
4. **Reconstruct daily points remaining** — for each day from `start_date` to today, sum points of items not in a completed state group as of that day. Use `list_work_item_activities` to find state transitions, or fall back to current snapshot if activity data is unavailable.
5. **Compute**:
   - `total_points` = sum of all cycle item points
   - `completed_points` = sum of items in completed state group
   - `remaining_points` = `total_points − completed_points`
   - `ideal_remaining(day)` = `total_points × (1 − day / total_days)`
   - `projected_end_points` = linear extrapolation from the last 3 days of actual burn
   - `on_track` = `projected_end_points ≤ 0`
6. **Present** — ASCII or markdown table with day, ideal, actual, delta. Highlight if the team is ahead/behind and by how much.
7. **Recommend** — if off track by > 20%, suggest scope reduction (see `sprint-planning` on transferring items).

## Example

```
/burndown "TaskFlow"
/burndown "TaskFlow" --cycle "Sprint 15"
```

## Output Format

```
Sprint 14 — Billing v2 (day 6 of 10)
Total: 40 pts · Completed: 22 · Remaining: 18 · Ideal: 16
Status: 🟡 2 pts behind ideal · Projected end: 30/40 (75%)

Day | Ideal | Actual | Delta
  0 |   40  |   40   |   0
  1 |   36  |   38   |  +2
  …
```

</details>

### `close-sprint`

Close current sprint — review completion, transfer incomplete items, archive

Arguments: `<project> [--transfer-to <next-cycle-name>]`

<details><summary>Prompt template</summary>

# Close Sprint

Close the active sprint — review completion metrics, handle incomplete items, and archive.

## Arguments
Format: `<project> [--transfer-to <next-cycle-name>]`
- project: Project name or identifier (required)
- --transfer-to: Name of next sprint to transfer incomplete items (optional)

Parse from "$ARGUMENTS".

## Process

0. **Bootstrap connector** — consult the `connector-bootstrap` skill. Probe `ToolSearch` for the action names referenced below (`list_projects`, `list_cycles`, etc.). Match tools by action suffix — never assume a specific MCP prefix. If multiple Plane instances are connected, ask the user which one to use. All formulas and rules come from the `agile-fundamentals` skill.

1. **Find active sprint:**
   ```
   list_cycles({ project_id })
   ```
   Find cycle where today is between start_date and end_date.

2. **Get sprint items:**
   ```
   list_cycle_work_items({ project_id, cycle_id })
   ```
   Categorize by state group: completed, started, unstarted.

3. **Calculate metrics:**
   - Total points planned
   - Points completed
   - Completion rate (%)
   - Items completed vs total

4. **Present sprint summary:**
   Show completed items, incomplete items, and metrics.

5. **Handle incomplete items:**
   If --transfer-to specified:
   ```
   list_cycles({ project_id })  // find or create target cycle
   transfer_cycle_work_items({ project_id, cycle_id, new_cycle_id })
   ```
   Otherwise, ask user what to do with incomplete items.

6. **Archive the sprint:**
   ```
   archive_cycle({ project_id, cycle_id })
   ```

7. **Display final summary:**
   Velocity recorded, items transferred (if any), sprint archived.

## Example Usage
```
/close-sprint "TaskFlow"
/close-sprint "My Project" --transfer-to "Sprint 13"
```

</details>

### `comment`

Add or list comments on a Plane work item

Arguments: `<action> <project> <item> [body]`

<details><summary>Prompt template</summary>

# Comment

Add or list comments on a Plane work item.

## Arguments

Format: `<action> <project> <item> [body]`

- `action`: `add` (default if omitted) | `list` | `update` | `delete`
- `project`: project name or identifier
- `item`: work item identifier (`PROJ-42`) or UUID
- `body`: comment text (for `add`/`update`) — supports markdown

Parse from `"$ARGUMENTS"`.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `create_work_item_comment`, `list_work_item_comments`, `update_work_item_comment`, `delete_work_item_comment`.
2. **Resolve project** → `project_id`. **Resolve work item** → `work_item_id`.
3. **Route**:
   - `add` → convert markdown body to HTML if the API expects `comment_html`; call `create_work_item_comment`. Mention the user via `@username` only if the API supports mentions — otherwise plain text.
   - `list` → `list_work_item_comments`, render newest-first with author + timestamp.
   - `update` → resolve `comment_id` (last by current user, or explicit `--id`), patch.
   - `delete` → confirm before destructive call.
4. **Confirm** — print the comment ID and a one-line preview.

## Examples

```
/comment add "TaskFlow" PROJ-148 "Reproduced on Safari 17.2 — root cause is the cookie SameSite change"
/comment list "TaskFlow" PROJ-148
/comment update "TaskFlow" PROJ-148 --id 9f2c... "Updated: also affects Safari 16"
/comment delete "TaskFlow" PROJ-148 --id 9f2c...
```

## Best Practices

- Comments are for **decisions and findings**, not status pings ("any update?"). Use standup or DM for that.
- When closing a bug, leave a final comment with: root cause, fix summary, test added. This is gold for future debugging.
- Cross-link related items in the body using their identifier (e.g. "see PROJ-150") — Plane auto-links them.

</details>

### `create-epic`

Create a Plane epic for a large feature spanning multiple sprints

Arguments: `<project> <epic name> [--lead <user>] [--target <YYYY-MM-DD>]`

<details><summary>Prompt template</summary>

# Create Epic

Create a new epic in Plane for a large feature or theme containing many work items.

## Arguments

Format: `<project> <epic name> [--lead <user>] [--target <YYYY-MM-DD>] [--priority <urgent|high|medium|low>]`

Parse from `"$ARGUMENTS"`.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `list_epics`, `create_epic`, `update_epic`.
2. **Resolve project** — `list_projects` → `project_id`.
3. **Resolve lead** — `get_project_members` → lead UUID.
4. **Create the epic** — follow the `epics-initiatives-milestones` skill. Call `create_epic` with `name`, `description_html` (goal + success metrics + out-of-scope), `lead`, `start_date`, `target_date`, `priority`.
5. **Offer to decompose** — ask if the user wants to run `/decompose` against the epic to create child work items now.
6. **Confirm** — print epic name, lead, target date, and identifier.

## When to Use an Epic

- Scope is 1–3 months of work
- Owned by a single tech lead
- Will be decomposed into many sprint-sized work items
- Not tied to a fixed release date (use a milestone for that)

## Example

```
/create-epic "TaskFlow" "Multi-tenant support" --lead alice --target 2026-07-01
/create-epic "TaskFlow" "Enterprise SSO" --lead bob --priority high
```

</details>

### `create-module`

Create a Plane module (feature/workstream grouping across sprints)

Arguments: `<project> <module name> [--lead <user>] [--target <YYYY-MM-DD>]`

<details><summary>Prompt template</summary>

# Create Module

Create a new module in Plane to group work items by feature or workstream across multiple sprints.

## Arguments

Format: `<project> <module name> [--lead <user>] [--target <YYYY-MM-DD>] [--description <text>]`

Parse from `"$ARGUMENTS"`.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `list_modules`, `create_module`, `add_work_items_to_module`.
2. **Resolve project** — `list_projects` → `project_id`.
3. **Resolve lead** — `get_project_members({ project_id })` → lead UUID.
4. **Create the module** — follow the `modules` skill. Call `create_module` with `name`, `description`, `lead`, `start_date`, `target_date`, `status: "planned"`.
5. **Offer to populate** — ask the user whether to add any existing backlog items to the module now (`add_work_items_to_module`).
6. **Confirm** — print module name, lead, target date, and item count.

## Example

```
/create-module "TaskFlow" "Billing v2" --lead alice --target 2026-06-30
/create-module "TaskFlow" "Onboarding revamp" --lead bob --target 2026-05-15 --description "Cut signup drop-off by 30%"
```

See the `modules` skill for when to use a module vs a cycle.

</details>

### `create-sprint`

Create a new sprint cycle in Plane

Arguments: `<project> <name> --start <YYYY-MM-DD> --end <YYYY-MM-DD>`

<details><summary>Prompt template</summary>

# Create Sprint

Quickly create a new sprint cycle with specified dates.

## Arguments
Format: `<project> <name> --start <YYYY-MM-DD> --end <YYYY-MM-DD>`
- project: Project name or identifier (required)
- name: Sprint name (required)
- --start: Start date in ISO format (required)
- --end: End date in ISO format (required)

Parse from "$ARGUMENTS".

## Process

0. **Bootstrap connector** — consult the `connector-bootstrap` skill. Probe `ToolSearch` for the action names referenced below (`list_projects`, `list_cycles`, etc.). Match tools by action suffix — never assume a specific MCP prefix. If multiple Plane instances are connected, ask the user which one to use. All formulas and rules come from the `agile-fundamentals` skill.

1. **Resolve project:**
   ```
   list_projects()
   ```
   Find project by name or identifier.

2. **Get current user:**
   ```
   get_me()
   ```
   Get user UUID for owned_by field.

3. **Create the cycle:**
   ```
   create_cycle({
     project_id, name, owned_by,
     start_date, end_date
   })
   ```

4. **Display result:**
   Show created sprint name, dates, and ID.
   Suggest next step: "Add items with /plan-sprint or manually."

## Example Usage
```
/create-sprint "TaskFlow" "Sprint 12" --start 2025-03-17 --end 2025-03-21
/create-sprint "My Project" "Sprint 5 — Auth" --start 2025-03-10 --end 2025-03-14
```

</details>

### `cycles`

Manage Plane cycles beyond planning — list, archive, transfer, delete

Arguments: `<action> <project> [args...]`

<details><summary>Prompt template</summary>

# Cycles

Operate on cycles (sprints) beyond create/plan/close. For sprint planning use `/plan-sprint` and `/create-sprint`; for status use `/sprint-status`; for closing use `/close-sprint`.

## Arguments

Format: `<action> <project> [args...]`

- `action`: `list` | `list-archived` | `get` | `archive` | `unarchive` | `transfer` | `delete` | `update`
- `project`: project name or identifier
- For `get`/`archive`/`unarchive`/`delete`: cycle name or ID
- For `transfer`: `--from <cycle>` `--to <cycle>` (incomplete items only by default; `--all` for everything)
- For `update`: `--name`, `--description`, `--start <date>`, `--end <date>`

Parse from `"$ARGUMENTS"`.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `list_cycles`, `list_archived_cycles`, `retrieve_cycle`, `update_cycle`, `delete_cycle`, `archive_cycle`, `unarchive_cycle`, `transfer_cycle_work_items`, `list_cycle_work_items`.
2. **Resolve project** → `project_id`.
3. **Route**:
   - `list` → `list_cycles`. Render with: name | state (upcoming/current/completed) | dates | items | progress.
   - `list-archived` → `list_archived_cycles`.
   - `get` → `retrieve_cycle` + `list_cycle_work_items`. Show metrics.
   - `archive` → confirm. Archived cycles disappear from default views but remain queryable.
   - `unarchive` → restore.
   - `transfer` → resolve both cycles. By default, transfer only items NOT in a `completed` group state. Call `transfer_cycle_work_items({ from_cycle_id, to_cycle_id, work_item_ids })`. Print count moved.
   - `delete` → **destructive**, confirm twice. Items lose cycle assignment but are not deleted. Prefer `archive` over `delete`.
   - `update` → resolve cycle → `update_cycle`. Changing dates of an active cycle distorts velocity history — warn the user.
4. **Confirm** — re-list after mutation.

## Examples

```
/cycles list "TaskFlow"
/cycles list-archived "TaskFlow"
/cycles get "TaskFlow" "Sprint 14"
/cycles transfer "TaskFlow" --from "Sprint 14" --to "Sprint 15"
/cycles archive "TaskFlow" "Sprint 10"
/cycles update "TaskFlow" "Sprint 15" --end 2026-04-25
/cycles delete "TaskFlow" "Sprint 09"
```

## Best Practices

- **Archive completed cycles after retro** — keeps the active list clean while preserving velocity data.
- Don't use `delete` to "redo" a sprint — historical data is gold for velocity. Archive instead.
- `transfer` should be the LAST step of a sprint close, after retro, not the first.

</details>

### `decompose`

Break down a work item into smaller stories/tasks using INVEST criteria

Arguments: `<work-item-identifier>`

<details><summary>Prompt template</summary>

# Decompose

Break down a work item into smaller, sprint-ready stories or tasks using INVEST criteria and vertical slicing.

## Arguments
Format: `<work-item-identifier>`
- work-item-identifier: Item identifier like "MP-42" (required)

Parse from "$ARGUMENTS". Extract project_identifier and issue_identifier number.

## Process

0. **Bootstrap connector** — consult the `connector-bootstrap` skill. Probe `ToolSearch` for the action names referenced below (`list_projects`, `list_cycles`, etc.). Match tools by action suffix — never assume a specific MCP prefix. If multiple Plane instances are connected, ask the user which one to use. All formulas and rules come from the `agile-fundamentals` skill.

1. **Retrieve the item:**
   ```
   retrieve_work_item_by_identifier({
     project_identifier: "<prefix>",
     issue_identifier: <number>
   })
   ```

2. **Analyze against INVEST criteria:**
   - Independent: check relations for hard dependencies
   - Negotiable: check description format
   - Valuable: check epic/business context
   - Estimable: check description detail
   - Small: check if points > 8
   - Testable: check acceptance criteria

3. **Propose decomposition:**
   Choose splitting pattern based on item type:
   - Workflow steps (most common)
   - Business rules
   - CRUD operations
   - Happy path vs edge cases
   - Data variations
   - Platform/interface

4. **Present proposed children:**
   | # | Child Item | Points | Rationale |
   |---|-----------|--------|-----------|

5. **On confirmation — create children:**
   ```
   For each child:
     create_work_item({
       project_id, name, parent: <parent_id>,
       point, priority, description_html
     })
   ```

6. **Set relations between children if needed:**
   ```
   create_work_item_relation({ relation_type: "blocked_by", ... })
   ```

## Example Usage
```
/decompose MP-42
/decompose TF-15
```

</details>

### `dependencies`

Show dependency graph and active blockers for a project or sprint

Arguments: `<project> [--cycle <current|next|name>]`

<details><summary>Prompt template</summary>

# Dependencies

Inspect work item relations to find active blockers, blocking chains, and risk items.

## Arguments

Format: `<project> [--cycle <current|next|name>] [--item <id>]`

Parse from `"$ARGUMENTS"`. Without `--cycle` or `--item`, scan the whole active backlog.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `list_work_items`, `list_work_item_relations`, `retrieve_work_item`.
2. **Resolve project** — `list_projects` → `project_id`.
3. **Scope the scan**:
   - `--cycle current` → items in the active cycle (`list_cycle_work_items`)
   - `--cycle <name>` → items in that cycle
   - `--item <id>` → just that item and its transitive relations
   - default → in-progress + unstarted items in the project
4. **For each item, fetch relations** — `list_work_item_relations({ project_id, work_item_id })`.
5. **Build the graph** — direction matters: `blocked_by` (incoming), `blocking` (outgoing), `duplicate`, `relates_to`.
6. **Highlight risk**:
   - Items blocked by something not yet started
   - Items blocking 2+ other items (critical path candidates)
   - Cyclic dependencies (report explicitly)
7. **Present** as a table grouped by risk level (🔴 blocker chain, 🟡 single blocker, 🟢 no blockers).

## Example

```
/dependencies "TaskFlow"
/dependencies "TaskFlow" --cycle current
/dependencies "TaskFlow" --item PROJ-148
```

## Output Format

```
🔴 Blocker chains
  PROJ-148 (Stripe webhook) ← blocks PROJ-152, PROJ-153, PROJ-160
    blocked_by PROJ-140 (backlog, unestimated)

🟡 Single blockers
  PROJ-155 blocked_by PROJ-147 (in progress, @alice)

🟢 Clear
  PROJ-150, PROJ-151, …
```

</details>

### `epic`

Manage Plane epics — list, get, update, delete (complement to /create-epic)

Arguments: `<action> <project> [args...]`

<details><summary>Prompt template</summary>

# Epic

Operate on epics beyond creation. For creation use `/create-epic`. For breaking an epic into stories use `/decompose`.

## Arguments

Format: `<action> <project> [args...]`

- `action`: `list` | `get` | `update` | `delete` | `status`
- `project`: project name or identifier
- For `get`/`update`/`status`/`delete`: epic name or ID
- For `update`: `--name`, `--description`, `--lead <user>`, `--target <date>`, `--initiative <name>`

Parse from `"$ARGUMENTS"`.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `list_epics`, `retrieve_epic`, `update_epic`, `delete_epic`. Note: child items of an epic are work items with `parent_id = epic_id`.
2. **Resolve project** → `project_id`.
3. **Route**:
   - `list` → render: name | lead | child count | state buckets | target | progress %.
   - `get` → details + child items grouped by state. Show progress as `(completed_points / total_points)` and as `(completed_count / total_count)`.
   - `status` → same as get but compact and emphasizes RAG (Red/Amber/Green) based on points completed vs time elapsed (see `epics-initiatives-milestones` skill).
   - `update` → patch. Linking to an initiative goes through this (`--initiative` → resolve initiative ID → set `initiative_id`).
   - `delete` → confirm. Children are unlinked, not deleted.
4. **Confirm** — re-render after mutation.

## Examples

```
/epic list "TaskFlow"
/epic get "TaskFlow" "Checkout Rewrite"
/epic status "TaskFlow" "Checkout Rewrite"
/epic update "TaskFlow" "Checkout Rewrite" --target 2026-06-30 --lead alice
/epic delete "TaskFlow" "Cancelled Idea"
```

## Best Practices

- Every epic needs an **outcome statement** in its description (not just a feature list). E.g. "Customers can complete checkout in <30s on mobile" — this is the success criterion.
- Epic health check: if an epic has been "in progress" for >3 cycles with <50% completion, rescope or split.
- Pair epics with a tracking page (`/page create project ... --from-template spec`) and link from the epic description.

</details>

### `estimate`

Estimate unestimated work items using Fibonacci story points

Arguments: `<project> [--cycle <cycle-name>] [--scale fibonacci|tshirt]`

<details><summary>Prompt template</summary>

# Estimate

Batch estimate unestimated work items in the backlog or a specific sprint.

## Arguments
Format: `<project> [--cycle <cycle-name>] [--scale fibonacci|tshirt]`
- project: Project name or identifier (required)
- --cycle: Specific sprint to estimate items in (optional, defaults to full backlog)
- --scale: Estimation scale — fibonacci (default) or tshirt

Parse from "$ARGUMENTS".

## Process

0. **Bootstrap connector** — consult the `connector-bootstrap` skill. Probe `ToolSearch` for the action names referenced below (`list_projects`, `list_cycles`, etc.). Match tools by action suffix — never assume a specific MCP prefix. If multiple Plane instances are connected, ask the user which one to use. All formulas and rules come from the `agile-fundamentals` skill.

1. **Resolve project:**
   ```
   list_projects()
   ```

2. **Get items to estimate:**
   If --cycle specified:
   ```
   list_cycles({ project_id }) → find cycle
   list_cycle_work_items({ project_id, cycle_id })
   ```
   Otherwise:
   ```
   list_work_items({ project_id })
   ```
   Filter: items where point is null or 0.

3. **Find reference stories (for calibration):**
   Look for completed items with known point values to anchor estimates.

4. **Estimate each item:**
   For each unestimated item:
   - Present: name, description, priority
   - Analyze: components touched, unknowns, dependencies
   - Suggest estimate with reasoning
   - If scale is tshirt: show XS/S/M/L/XL then map to Fibonacci

5. **On confirmation per item:**
   ```
   update_work_item({ project_id, work_item_id, point: <value> })
   ```

6. **Flag oversized items:**
   Items > 8 points → suggest decomposition with /decompose.

7. **Summary:**
   Total items estimated, total points, items flagged for splitting.

## Example Usage
```
/estimate "TaskFlow"
/estimate "My Project" --cycle "Sprint 12"
/estimate "ShopFlow" --scale tshirt
```

</details>

### `find`

Search Plane work items by query, identifier, or filter

Arguments: `<query> [--project name] [--state name] [--assignee user] [--label name]`

<details><summary>Prompt template</summary>

# Find

Quick search for Plane work items. Accepts a free-text query, an identifier (`PROJ-42`), or filter flags.

## Arguments

Format: `<query> [flags...]`

- `query`: free text, OR an identifier like `PROJ-42`, OR omit when using only filters
- `--project <name>`: scope to a project
- `--state <name>`: filter by state (e.g. "In Progress")
- `--assignee <user>`: filter by assignee (or `me`)
- `--label <name>`: filter by label
- `--limit <n>`: max results (default 20)

Parse from `"$ARGUMENTS"`.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `search_work_items`, `retrieve_work_item_by_identifier`, `list_work_items`, `get_workspace_members`, `get_me`.
2. **Detect identifier shortcut** — if the query matches `^[A-Z][A-Z0-9]+-\d+$`, split into project slug + integer and call `retrieve_work_item_by_identifier` directly. Render the full item and stop.
3. **Free-text path** — call `search_work_items({ query })` (workspace-scoped on most builds). If a `--project` is given and the API doesn't support project filter, post-filter results client-side.
4. **Filter-only path** — if no query but flags are present, use `list_work_items` with filter parameters.
5. **Apply remaining filters** client-side (state, assignee, label) — these are not always supported by `search_work_items`.
6. **Render** — table with columns: `ID | Title | State | Assignee | Updated`. Truncate titles at 60 chars. Show top `--limit` results.
7. **Suggest next actions** — for any single result, hint at `/work-item get`, `/comment add`, `/assign`.

## Examples

```
/find "login bug"
/find PROJ-148
/find --assignee me --state "In Progress"
/find "checkout" --project "TaskFlow" --label type/bug
/find --label priority/p0 --limit 5
```

## Best Practices

- For frequently-used filters (e.g. "my open p0 bugs"), save them as a Plane view and reference the view name in `/find` instead.
- Search is full-text over title and description on most Plane builds — comments are usually NOT indexed.
- If results feel stale, the workspace search index may be lagging — fall back to `list_work_items` with explicit filters.

</details>

### `groom-backlog`

Run a backlog grooming session — refine, estimate, and prioritize

Arguments: `<project> [--limit <n>]`

<details><summary>Prompt template</summary>

# Groom Backlog

Run a structured backlog grooming session: review top items, fill missing details, estimate unestimated ones, and reprioritize.

## Arguments

Format: `<project> [--limit <n>]`

Parse from `"$ARGUMENTS"`. Default limit: top 20 items by priority.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `list_work_items`, `update_work_item`, `list_work_item_relations`.
2. **Resolve project** — `list_projects` → `project_id`.
3. **Load candidate items** — `list_work_items({ project_id, state_group: "backlog", order_by: "-priority" })`.
4. **For each item, check Definition of Ready** (see `agile-fundamentals`):
   - Title and description with acceptance criteria
   - Estimated (points 1–8)
   - No unresolved `blocked_by`
   - Assignable
5. **Fix gaps**:
   - Missing AC → ask user or draft and confirm
   - Unestimated → run `estimation` skill flow
   - Too large (> 8 points) → run `task-decomposition` skill
   - Stale (not updated > 30 days) → flag for archive or re-confirm relevance
6. **Reprioritize** — use `backlog-management` (MoSCoW) or run `/wsjf-prioritize` if numeric scoring is needed.
7. **Present grooming summary** — what was refined, estimated, decomposed, archived.

## Example

```
/groom-backlog "TaskFlow"
/groom-backlog "TaskFlow" --limit 30
```

## Cadence

- Weekly: 30–60 min session
- Before every sprint planning: quick pass over top capacity × 1.5 worth of items

</details>

### `history`

Show the change history (activity log) of a Plane work item

Arguments: `<project> <item> [--limit n]`

<details><summary>Prompt template</summary>

# History

Render the activity timeline of a work item — every state change, assignment, label, comment, link, and field edit.

## Arguments

Format: `<project> <item> [--limit n]`

- `project`: project name or identifier
- `item`: work item identifier (`PROJ-42`) or UUID
- `--limit <n>`: max events (default 50, newest-first)
- `--since <date>`: only events after a given date

Parse from `"$ARGUMENTS"`.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `list_work_item_activities`, `retrieve_work_item_activity`, `retrieve_work_item_by_identifier`.
2. **Resolve project** → `project_id`. **Resolve work item** → `work_item_id`.
3. **Fetch** — `list_work_item_activities({ project_id, work_item_id })`. Apply `--since` filter and `--limit`.
4. **Normalize each event** — Plane activities have a `verb` (`created`, `updated`, etc.) and a `field`. Render as one line:
   - `2026-04-08 14:32 alice  state: To Do → In Progress`
   - `2026-04-08 14:35 alice  +label: type/bug`
   - `2026-04-08 14:40 bob    comment: "Reproduced..."`
   - `2026-04-08 15:01 alice  assignees: +charlie`
5. **Render** — newest first. Group consecutive events from the same user within 5 minutes into a single block.
6. **Highlight** — mark state regressions in red (e.g., `In Review → In Progress`) — these are leak indicators worth investigating.

## Examples

```
/history "TaskFlow" PROJ-148
/history "TaskFlow" PROJ-148 --limit 20
/history "TaskFlow" PROJ-148 --since 2026-04-01
```

## Best Practices

- Use during retros to figure out *why* an item slipped — when did it stall, who picked it up, when did the blocker land.
- Use for bug forensics — find when severity was changed, or when a label got removed.
- State regressions are a leak signal. If they happen often, your Definition of Done is too loose (see `agile-fundamentals`).

</details>

### `initiative`

Manage Plane initiatives — strategic groupings above epics

Arguments: `<action> [args...]`

<details><summary>Prompt template</summary>

# Initiative

Initiatives are the highest-level planning unit in Plane — they group multiple epics across multiple projects under a single strategic theme. See the `epics-initiatives-milestones` skill for the conceptual model.

## Arguments

Format: `<action> [args...]`

- `action`: `list` | `create` | `update` | `get` | `delete` | `link-epic`
- For `create`: `--name`, `--description`, `--lead <user>`, `--start <date>`, `--target <date>`
- For `link-epic`: `--initiative <name|id>`, `--epic <project>:<id>`

Parse from `"$ARGUMENTS"`.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `list_initiatives`, `create_initiative`, `update_initiative`, `retrieve_initiative`, `delete_initiative`. Initiatives are workspace-scoped (no `project_id`).
2. **Route**:
   - `list` → `list_initiatives`. Render: name | lead | epics count | start–target | status.
   - `create` → gather name, description, dates. Initiatives without an explicit lead and target date drift — warn the user if either is missing.
   - `update` → resolve by name or ID → `update_initiative`.
   - `get` → `retrieve_initiative` + render its epics list (cross-project) and aggregate progress.
   - `delete` → confirm. Linked epics are NOT deleted, just unlinked.
   - `link-epic` → many Plane builds use the epic's `initiative_id` field via `update_epic`. If a dedicated `add_epic_to_initiative` tool exists, use it; otherwise patch the epic.
3. **Confirm** — print initiative ID and a summary of linked epics.

## When to use what

| Unit | Scope | Time horizon | Owner |
|---|---|---|---|
| Initiative | workspace, multi-project | quarter / multi-quarter | exec / PM lead |
| Epic | single project | weeks–months | tech lead |
| Module | single project, scope-boxed | flexible | feature owner |
| Cycle | single project, time-boxed | 1–2 weeks | scrum master |
| Milestone | release marker | event date | release manager |

If your work fits inside one project, use an **epic**, not an initiative. Initiatives are for cross-team strategic bets.

## Examples

```
/initiative list
/initiative create --name "Q2 Mobile Launch" --lead alice --target 2026-06-30
/initiative get "Q2 Mobile Launch"
/initiative link-epic --initiative "Q2 Mobile Launch" --epic "Mobile App":42
/initiative update "Q2 Mobile Launch" --target 2026-07-15
```

</details>

### `label`

Manage Plane labels — list, create, update, delete, or apply to a work item

Arguments: `<action> <project> [args...]`

<details><summary>Prompt template</summary>

# Label

Manage labels in a Plane project. Labels drive triage, grooming, and reporting filters.

## Arguments

Format: `<action> <project> [args...]`

- `action`: `list` | `create` | `update` | `delete` | `apply` | `remove`
- `project`: project name or identifier
- Remaining: label name, `--color #hex`, `--description`, `--item PROJ-42`

Parse from `"$ARGUMENTS"`.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `list_labels`, `create_label`, `update_label`, `delete_label`, `retrieve_label`, plus `update_work_item` (for `apply`/`remove`).
2. **Resolve project** — `list_projects` → `project_id`.
3. **Route**:
   - `list` → `list_labels({ project_id })`. Group by prefix (`type/`, `area/`, `priority/`) for readability.
   - `create` → `create_label({ project_id, name, color, description })`. If color omitted, pick from a stable palette by hashing the name.
   - `update` → resolve label by name → `update_label`.
   - `delete` → confirm with the user before destructive call (labels in use are unlinked from items).
   - `apply` → resolve work item via `retrieve_work_item_by_identifier`, fetch current `label_ids`, append the new one, `update_work_item({ label_ids })`. Check schema field name — some Plane builds use `labels`.
   - `remove` → same but remove from the list.
4. **Confirm** — print resulting label set or item label list.

## Conventions

Recommend a **prefixed taxonomy** so labels stay searchable:

- `type/bug`, `type/feature`, `type/chore`, `type/spike`, `type/tech-debt`
- `area/api`, `area/web`, `area/mobile`, `area/infra`
- `priority/p0`, `priority/p1`, `priority/p2`
- `status/needs-info`, `status/blocked`, `status/ready-for-review`
- `customer/acme`, `customer/contoso` (only if you track per-customer work)

Avoid creating one-off labels — they become noise. Audit with `/label list` quarterly.

## Examples

```
/label list "TaskFlow"
/label create "TaskFlow" "type/bug" --color #e11d48
/label update "TaskFlow" "type/bug" --description "Defect in shipped behavior"
/label apply "TaskFlow" --item PROJ-148 "type/bug"
/label remove "TaskFlow" --item PROJ-148 "status/needs-info"
/label delete "TaskFlow" "old-label"
```

</details>

### `link`

Add or list external links (PRs, docs, designs) on a Plane work item

Arguments: `<action> <project> <item> [url] [title]`

<details><summary>Prompt template</summary>

# Link

Attach external URLs to a work item — pull requests, design docs, Figma boards, runbooks.

## Arguments

Format: `<action> <project> <item> [url] [title]`

- `action`: `add` (default) | `list` | `remove`
- `project`: project name or identifier
- `item`: work item identifier or UUID
- `url`: full URL (required for `add`)
- `title`: human-readable label (optional; auto-derive from URL host + path tail if omitted)
- `--id <link_id>` for `remove`

Parse from `"$ARGUMENTS"`.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `create_work_item_link`, `list_work_item_links`, `delete_work_item_link`.
2. **Resolve project** → `project_id`. **Resolve work item** → `work_item_id`.
3. **Route**:
   - `add` → if title omitted, derive: `github.com/org/repo/pull/420` → `"PR #420"`, `figma.com/file/...` → `"Figma design"`, `docs.google.com/...` → `"Google Doc"`. Call `create_work_item_link({ url, title, ... })`.
   - `list` → `list_work_item_links`, render as `Title — URL`.
   - `remove` → with `--id`, call `delete_work_item_link`. Without, list and ask which to remove.
4. **Confirm** — print link title and the item identifier.

## Examples

```
/link add "TaskFlow" PROJ-148 https://github.com/org/repo/pull/420
/link add "TaskFlow" PROJ-148 https://figma.com/file/abc "Login flow design v3"
/link list "TaskFlow" PROJ-148
/link remove "TaskFlow" PROJ-148 --id 9f2c-...
```

## Best Practices

- Always link the **PR** to the work item — this is how reviewers find context and how release-notes generation finds the change.
- Link **design** before implementation, **runbook** before deploy.
- Avoid pasting links into the description body — use proper links so they can be enumerated.

</details>

### `log-time`

Log work time on a Plane work item (create a work log entry)

Arguments: `<project> <item> <duration> [description]`

<details><summary>Prompt template</summary>

# Log Time

Create a work log entry against a Plane work item. Time is stored as **integer minutes** in the API — convert human input.

## Arguments

Format: `<project> <item> <duration> [description]`

- `project`: project name or identifier
- `item`: work item identifier (e.g. `PROJ-42`) or UUID
- `duration`: human form (`2h`, `2h30m`, `45m`, `1.5h`, `PT2H30M`) — convert to integer minutes
- `description` (optional): free text — what was done

Parse from `"$ARGUMENTS"`.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `create_work_log`, `list_work_logs`, `get_project_worklog_summary`.
2. **Verify time tracking is enabled** — check `get_project_features` or fetch project; if `is_time_tracking_enabled` is false, tell the user how to enable it (project settings → features) and stop.
3. **Resolve project** → `project_id`. **Resolve work item** → if identifier like `PROJ-42`, use `retrieve_work_item_by_identifier`; if UUID, skip.
4. **Convert duration to minutes**:
   - `2h` → 120, `2h30m` → 150, `45m` → 45, `1.5h` → 90, `PT2H30M` → 150
   - Reject if result < 1 or > 24h in a single entry (likely a typo); ask user to confirm.
5. **Create the log** — `create_work_log({ project_id, work_item_id, duration: <minutes>, description })`.
6. **Confirm** — print: total logged today on this item, total logged this sprint by this user (use `list_work_logs` filtered).

## Subcommands

The base form logs time. Additional verbs (parse `<verb> ...` if first arg is one of these):

- `summary <project>` → `get_project_worklog_summary` — total logged per user / per item
- `list <project> <item>` → `list_work_logs` for that item
- `delete <project> <log_id>` → `delete_work_log` (confirm first)
- `update <project> <log_id> <duration>` → `update_work_log`

## Examples

```
/log-time "TaskFlow" PROJ-148 2h "Debug Safari login bug + fix"
/log-time "TaskFlow" PROJ-148 45m
/log-time summary "TaskFlow"
/log-time list "TaskFlow" PROJ-148
/log-time delete "TaskFlow" 9f2c-...
```

## Best Practices

- Log time **at the end of each work session**, not in batch at week's end — accuracy drops sharply after 24h.
- One entry per session per item. Don't merge a whole day into one log.
- Use `/log-time summary` before estimation sessions — actual minutes per story point is the most honest velocity input (see `velocity-metrics` skill).

</details>

### `members`

List Plane workspace or project members and their roles

Arguments: `[scope] [project]`

<details><summary>Prompt template</summary>

# Members

List members of the workspace or a specific project.

## Arguments

Format: `[scope] [project]`

- `scope`: `workspace` (default) | `project`
- `project`: required when `scope=project` — project name or identifier
- `--role <admin|member|viewer|guest>`: filter
- `--active`: only active members

Parse from `"$ARGUMENTS"`.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `get_workspace_members`, `get_project_members`, `get_me`.
2. **Route**:
   - `workspace` → `get_workspace_members`. Render: name | email | role | joined | last active.
   - `project` → resolve `project_id` → `get_project_members`. Add a column showing the workspace role for context.
3. **Apply filters** client-side.
4. **Highlight current user** (from `get_me`).
5. **Footer** — total count, count by role.

## Examples

```
/members
/members workspace --role admin
/members project "TaskFlow"
/members project "TaskFlow" --role member
```

## Best Practices

- Audit workspace admins quarterly — too many admins is a security risk.
- Project membership should be the minimum needed; use viewer for stakeholders.
- Pair with `/assign` to find the right user when names are ambiguous.

</details>

### `milestone-status`

Show status and risk for a Plane milestone (release/launch tracking)

Arguments: `<project> [milestone name or id]`

<details><summary>Prompt template</summary>

# Milestone Status

Report the health of a milestone: remaining work, velocity-based ETA, and Red/Yellow/Green risk.

## Arguments

Format: `<project> [milestone name or id]`

Parse from `"$ARGUMENTS"`. If no milestone is specified, list all active milestones with their status.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `list_milestones`, `list_milestone_work_items`, `retrieve_milestone`.
2. **Resolve project and milestone** — `list_projects`, then `list_milestones({ project_id })`.
3. **Load milestone items** — `list_milestone_work_items({ project_id, milestone_id })`.
4. **Compute metrics** — follow the reporting section of `epics-initiatives-milestones`:
   - `total_points`, `completed_points`, `remaining_points`
   - Weekly velocity from recent cycles (see `velocity-metrics` skill)
   - ETA: `remaining_points / weekly_velocity`
   - Days to `target_date`
5. **Classify risk** per `agile-fundamentals`:
   - Green: ETA + 20% buffer < target_date
   - Yellow: ETA ≤ target_date, buffer < 20%
   - Red: ETA > target_date
6. **Present** — table with milestone name, target, completion, ETA, risk. Highlight red items first.

## Example

```
/milestone-status "TaskFlow"
/milestone-status "TaskFlow" "v2.0 Public Beta"
```

## Output Format

```
| Milestone | Target | Completion | ETA | Risk |
|-----------|--------|-----------|-----|------|
| v2.0 Beta | Apr 30 | 34/52 (65%) | Apr 28 | 🟢 |
| v2.1      | May 15 | 10/48 (21%) | May 22 | 🔴 |
```

</details>

### `milestone`

Manage Plane milestones — create, list, add items, update, delete

Arguments: `<action> <project> [args...]`

<details><summary>Prompt template</summary>

# Milestone

Create and manage milestones (release markers) on a Plane project. For status/risk reporting on an existing milestone use `/milestone-status`.

## Arguments

Format: `<action> <project> [args...]`

- `action`: `create` | `list` | `get` | `update` | `delete` | `add-items` | `remove-items`
- `project`: project name or identifier
- For `create`: `--name`, `--target <date>`, `--description`
- For `add-items`/`remove-items`: `--milestone <name>`, item IDs as remaining args

Parse from `"$ARGUMENTS"`.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `create_milestone`, `list_milestones`, `retrieve_milestone`, `update_milestone`, `delete_milestone`, `add_work_items_to_milestone`, `remove_work_items_from_milestone`, `list_milestone_work_items`.
2. **Resolve project** → `project_id`.
3. **Route**:
   - `create` → gather name + target date (required) + description. Call `create_milestone`. Reject milestones >6 months out — split into two.
   - `list` → `list_milestones({ project_id })`, render with item count and target date.
   - `get` → `retrieve_milestone` + `list_milestone_work_items`. Render scope and aggregate state buckets.
   - `update` → resolve by name → `update_milestone`. Moving the target date is a **trade-off conversation** — reduce scope first, slip date last (see `epics-initiatives-milestones` skill).
   - `delete` → confirm. Items are unlinked, not deleted.
   - `add-items` / `remove-items` → resolve item IDs → `add_work_items_to_milestone({ milestone_id, work_item_ids: [...] })`.
4. **Confirm** — print milestone ID, target date, and item count.

## Examples

```
/milestone create "TaskFlow" --name "v2.0 Public Beta" --target 2026-06-15
/milestone list "TaskFlow"
/milestone get "TaskFlow" "v2.0 Public Beta"
/milestone add-items "TaskFlow" --milestone "v2.0 Public Beta" PROJ-148 PROJ-150 PROJ-151
/milestone update "TaskFlow" "v2.0 Public Beta" --target 2026-06-22
/milestone remove-items "TaskFlow" --milestone "v2.0 Public Beta" PROJ-160
```

## Best Practices

- Every milestone needs a **single owner** (the release manager).
- Lock the milestone scope at T-2 weeks. Add-items after lock requires removing equivalent points.
- Pair `/milestone create` with a Plane page (`/page create project ... --from-template milestone-update`) and link them in the milestone description.
- Use `/milestone-status` weekly during the run-up to track risk.

</details>

### `module`

Manage Plane modules — list, get, update, add items, archive (complement to /create-module)

Arguments: `<action> <project> [args...]`

<details><summary>Prompt template</summary>

# Module

Operate on modules (feature workstreams) beyond creation. For initial creation use `/create-module`.

## Arguments

Format: `<action> <project> [args...]`

- `action`: `list` | `list-archived` | `get` | `update` | `add-items` | `remove-items` | `archive` | `unarchive` | `delete`
- `project`: project name or identifier
- For `get`/`update`/`add-items`/etc: module name or ID
- For `update`: `--name`, `--description`, `--lead <user>`, `--target <date>`, `--status <planned|in-progress|paused|completed|cancelled>`
- For `add-items`/`remove-items`: item identifiers as remaining args

Parse from `"$ARGUMENTS"`.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `list_modules`, `list_archived_modules`, `retrieve_module`, `update_module`, `delete_module`, `archive_module`, `unarchive_module`, `add_work_items_to_module`, `remove_work_item_from_module`, `list_module_work_items`.
2. **Resolve project** → `project_id`. Resolve module by name when given.
3. **Route**:
   - `list` → render: name | lead | items | state buckets | target | progress
   - `get` → details + items list grouped by state
   - `update` → patch
   - `add-items` / `remove-items` → resolve item UUIDs, call the corresponding tool. Note: `add` typically takes an array; `remove` is often single-item — may need a loop.
   - `archive` / `unarchive` / `delete` → confirm before delete; prefer archive
4. **Confirm** — re-render the module after mutation.

## Modules vs Cycles

| Cycles | Modules |
|---|---|
| time-boxed (sprints) | scope-boxed (features) |
| 1–2 weeks | weeks–months |
| velocity unit | progress unit |
| every item belongs to one cycle | items can belong to multiple modules |

A single feature usually spans 2–4 cycles. Use both: items live in a cycle for *when* and a module for *what*.

## Examples

```
/module list "TaskFlow"
/module get "TaskFlow" "Checkout v2"
/module update "TaskFlow" "Checkout v2" --status in-progress --target 2026-05-30
/module add-items "TaskFlow" "Checkout v2" PROJ-148 PROJ-149 PROJ-150
/module remove-items "TaskFlow" "Checkout v2" PROJ-148
/module archive "TaskFlow" "Old Workstream"
```

</details>

### `my-work`

List Plane work items assigned to the current user

Arguments: `[--project name] [--state name] [--cycle current|next|all] [--limit n]`

<details><summary>Prompt template</summary>

# My Work

Show open work items assigned to the current user across all projects, or filtered.

## Arguments

Format: `[flags...]`

- `--project <name>`: scope to one project (default: all projects in the workspace)
- `--state <group|name>`: filter (default: anything not in `completed` or `cancelled` group)
- `--cycle current|next|all`: filter by cycle membership (default: all)
- `--priority <p0|p1|p2|...>`
- `--limit <n>`: max items (default 25)
- `--all`: include completed and cancelled

Parse from `"$ARGUMENTS"`.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `get_me`, `list_projects`, `list_work_items`, `list_cycle_work_items`, `list_states`.
2. **Resolve current user** — `get_me` → `user_id`.
3. **Resolve scope**:
   - if `--project` → that one `project_id`
   - else → `list_projects` → loop over each
4. **Fetch items** — `list_work_items({ project_id, assignees: [user_id], ... })`. If the API does not accept assignee filter, fetch and filter client-side.
5. **Apply filters** — drop completed/cancelled (unless `--all`), apply state/priority/cycle filters. For `--cycle current`, look up the active cycle via `list_cycles({ project_id })` and filter against `list_cycle_work_items`.
6. **Render** — group by project, sort by priority then state. Columns: `ID | Title | State | Priority | Cycle | Updated`.
7. **Show summary** — total items, total story points, breakdown by state, count of overdue.
8. **Suggest** — `/log-time` for items in progress, `/comment add` for items idle >3 days.

## Examples

```
/my-work
/my-work --cycle current
/my-work --project "TaskFlow" --priority p0
/my-work --state "In Review"
/my-work --all --limit 50
```

## Best Practices

- Run this every morning before standup.
- WIP > 3 in-progress items is a red flag (see `agile-fundamentals` WIP limits).
- If the same item shows up day after day with no movement, it's stuck — comment why or ask for help.

</details>

### `page`

Create, list, get, or update a Plane page (project or workspace scope)

Arguments: `<action> [scope] [project] [args...]`

<details><summary>Prompt template</summary>

# Page

Manage Plane Pages — long-form documents attached to a project or to the workspace. Delegates rendering rules to the `pages-publishing` skill and tool resolution to `connector-bootstrap`.

## Arguments

Format: `<action> [scope] [project] [args...]`

- `action`: `create` | `list` | `get` | `update`
- `scope`: `project` (default) | `workspace`
- `project`: project name or identifier (required when `scope=project`)
- Remaining: title, content source (`--from-file`, `--from-template`, inline body), `--page-id` for `get`/`update`

Parse from `"$ARGUMENTS"`.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe `ToolSearch` for: `create_project_page`, `retrieve_project_page`, `create_workspace_page`, `retrieve_workspace_page`. Note: list and update endpoints may not exist on every Plane build — fall back gracefully and tell the user.
2. **Resolve scope**:
   - `project` → `list_projects` → `project_id`
   - `workspace` → no project lookup; uses workspace-level call
3. **Route by action**:
   - `create` → gather `name` and `description_html`. Sources of body, in order of preference:
     - `--from-file <path>` → read file, convert markdown→HTML if needed
     - `--from-template <name>` → use a template from `pages-publishing` skill (`sprint-report`, `retro`, `release-notes`, `roadmap`, `milestone-update`, `meeting-notes`, `decision-log`, `spec`, `runbook`, `blank`)
     - inline body in arguments → wrap in `<p>` paragraphs
     - none → ask the user once for the body
     Call `create_project_page` or `create_workspace_page`.
   - `list` → if a list tool is exposed by the connector, use it; otherwise tell the user pages must be opened from the Plane UI sidebar and offer to retrieve a known page by ID.
   - `get` → `retrieve_*_page({ page_id })`. Render the HTML back as readable markdown for the terminal.
   - `update` → not all Plane builds expose update; if unavailable, instruct the user to recreate the page or edit in UI. If available, patch `description_html`.
4. **Confirm** — print the page name, ID, and URL. Suggest where to link it (cycle description, Slack, README).

## Examples

```
/page create project "TaskFlow" "Q2 Engineering Plan" --from-file ./plan.md
/page create project "TaskFlow" "Sprint 14 Report" --from-template sprint-report
/page create workspace "Team Handbook"
/page get project "TaskFlow" --page-id 7c8e...
/page update project "TaskFlow" --page-id 7c8e... --from-file ./plan-v2.md
/page list project "TaskFlow"
```

## Best Practices

- Project pages live with the project they document — use them for sprint reports, retros, decision logs, runbooks.
- Workspace pages are for cross-project content — handbooks, OKRs, hiring loops.
- Always include a "Last updated" line at the top — Plane does not surface freshness in the sidebar.
- Prefer **editing** an existing page (e.g. roadmap) over recreating it; recreating breaks links.

</details>

### `plan-sprint`

Plan a new sprint — calculate capacity, select items from backlog, set sprint goal

Arguments: `<project> [--duration <days>] [--goal <sprint goal>]`

<details><summary>Prompt template</summary>

# Plan Sprint

Run a complete sprint planning ceremony — capacity calculation, backlog selection, and sprint creation.

## Arguments
Format: `<project> [--duration <days>] [--goal <sprint goal>]`
- project: Project name or identifier (required)
- --duration: Sprint length in days (default: 5)
- --goal: Sprint goal description (optional, will be suggested)

Parse from "$ARGUMENTS".

## Process

0. **Bootstrap connector** — consult the `connector-bootstrap` skill. Probe `ToolSearch` for the action names referenced below (`list_projects`, `list_cycles`, etc.). Match tools by action suffix — never assume a specific MCP prefix. If multiple Plane instances are connected, ask the user which one to use. All formulas and rules come from the `agile-fundamentals` skill.

1. **Resolve project:**
   ```
   list_projects()
   ```
   Find project by name or identifier. Get project_id.

2. **Get team and context:**
   ```
   get_project_members({ project_id })
   get_me()
   list_states({ project_id })
   ```
   Count team members. Get current user ID. Map state names to UUIDs.

3. **Calculate velocity:**
   ```
   list_archived_cycles({ project_id })
   ```
   For last 3-5 archived cycles, get work items and sum completed points.
   Calculate average velocity.

4. **Calculate capacity:**
   ```
   With history: capacity = avg_velocity × 0.85
   Without history: capacity = team_size × duration × 0.7 × 0.85
   ```

5. **Select backlog items:**
   ```
   list_work_items({ project_id, order_by: "-priority" })
   ```
   Filter: state group = backlog/unstarted, point is set.
   Select items by priority until capacity reached.

6. **Present proposed sprint:**
   Show table with items, points, assignees, total vs capacity.
   Suggest sprint goal based on selected items.

7. **On confirmation — create sprint:**
   ```
   create_cycle({ project_id, name, owned_by, start_date, end_date, description })
   add_work_items_to_cycle({ project_id, cycle_id, issue_ids })
   ```

8. **Verify:**
   ```
   list_cycle_work_items({ project_id, cycle_id })
   ```
   Confirm all items are in the sprint.

## Example Usage
```
/plan-sprint "My Project"
/plan-sprint "TaskFlow" --duration 5 --goal "Launch user authentication"
```

</details>

### `projects`

List, inspect, update, or delete Plane projects (complement to /setup-project)

Arguments: `<action> [args...]`

<details><summary>Prompt template</summary>

# Projects

Operate on Plane projects. For initial creation with Agile defaults use `/setup-project`.

## Arguments

Format: `<action> [args...]`

- `action`: `list` | `get` | `update` | `delete` | `features`
- For `get`/`update`/`delete`/`features`: project name or identifier
- For `update`: `--name`, `--identifier <SLUG>`, `--description`, `--icon`, `--lead <user>`, `--default-assignee <user>`
- For `features`: `--enable cycles,modules,issues,inbox,pages,views` or `--disable ...`

Parse from `"$ARGUMENTS"`.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `list_projects`, `retrieve_project`, `update_project`, `delete_project`, `get_project_features`, `update_project_features`, `get_project_members`.
2. **Route**:
   - `list` → render: identifier | name | lead | members | cycles? | modules? | last activity. Highlight archived.
   - `get` → details + member count + active cycle/module count.
   - `update` → patch. Changing `identifier` rewrites all work item IDs (`OLD-42` → `NEW-42`) — warn loudly before doing this.
   - `delete` → **destructive**. Confirm with project name typed back. Suggest archive in UI as the safer alternative.
   - `features` → `get_project_features` first, show current state, then `update_project_features`. Common flags: cycles, modules, issues, inbox (intake), pages, views, time-tracking.
3. **Confirm** — print result.

## Examples

```
/projects list
/projects get "TaskFlow"
/projects update "TaskFlow" --lead alice --description "Customer-facing checkout app"
/projects features "TaskFlow" --enable inbox,pages,modules
/projects delete "Old Sandbox"
```

## Best Practices

- Don't rename project `identifier` after launch — every existing reference (commits, PR titles, Slack threads) breaks.
- Disable unused features — clutter in the sidebar slows everyone down. Most teams need: cycles, modules, pages, views.
- Use `/projects features` to enable `inbox` (intake) before running `/triage-intake`.

</details>

### `property`

Manage custom properties on Plane work items (severity, customer impact, etc.)

Arguments: `<action> <project> [args...]`

<details><summary>Prompt template</summary>

# Property

Define and manage custom properties on work items. Properties are typed fields beyond the built-ins (state, priority, points). Examples: severity, customer impact, environment, root cause.

## Arguments

Format: `<action> <project> [args...]`

- `action`: `list` | `create` | `update` | `delete` | `get`
- `project`: project name or identifier
- For `create`: `--name`, `--type <text|number|select|multi-select|date|boolean|user|url>`, `--required`, `--type-id <work_item_type_id>` (scope to a specific work item type), `--options "a,b,c"` (for select)
- For `update`: same flags

Parse from `"$ARGUMENTS"`.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `list_work_item_properties`, `create_work_item_property`, `update_work_item_property`, `delete_work_item_property`, `retrieve_work_item_property`.
2. **Resolve project** → `project_id`. If `--type-id` not given, resolve work item type by name.
3. **Route**:
   - `list` → render: name | data type | scope (type) | required? | option count
   - `create` → for `select`/`multi-select`, parse options. Call `create_work_item_property`.
   - `update` → resolve by name. **Changing data type breaks existing values** — warn loudly.
   - `delete` → confirm. Existing values are dropped.
   - `get` → details + sample of items that have this property set.
4. **Confirm** — re-list.

## Property design rules

- **Use a property when**: data is structured, queryable, and reportable (severity, environment, customer).
- **Use a label when**: data is freeform tagging, optional, no values needed (`needs-design`, `flaky`).
- **Use a work item type when**: the workflow itself differs (Bug vs Spike).

Three buckets, three tools — don't mix. See `labels-states-properties` skill.

## Examples

```
/property list "TaskFlow"
/property create "TaskFlow" --name severity --type select --options "S0,S1,S2,S3" --required
/property create "TaskFlow" --name "customer impact" --type number
/property create "TaskFlow" --name environment --type select --options "prod,staging,dev"
/property update "TaskFlow" severity --required false
/property delete "TaskFlow" "old field"
```

</details>

### `publish-report`

Publish a Plane page — sprint report, retro, release notes, or roadmap

Arguments: `<type> <project> [args...]`

<details><summary>Prompt template</summary>

# Publish Report

Generate and publish a Plane project page using a template from the `pages-publishing` skill.

## Arguments

Format: `<type> <project> [args...]`

- `type`: `sprint-report` | `retro` | `release-notes` | `roadmap` | `milestone-update`
- `project`: project name or identifier
- Remaining args depend on type

Parse from `"$ARGUMENTS"`.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `create_project_page`, `create_workspace_page`, and the data-gathering actions required by the chosen type.
2. **Resolve project** — `list_projects` → `project_id`.
3. **Gather data** based on `type`:
   - `sprint-report` → active or last cycle, items, metrics (see `velocity-metrics`)
   - `retro` → previous sprint items, previous retro page (if exists), attendees
   - `release-notes` → items closed in a version range, grouped by label
   - `roadmap` → active cycles, modules, epics, milestones
   - `milestone-update` → target milestone and its items (see `epics-initiatives-milestones`)
4. **Render HTML** using the matching template from the `pages-publishing` skill.
5. **Create the page** — `create_project_page({ project_id, name, description_html })`.
6. **Share** — return the page URL and suggest where to link it (cycle description, Slack, etc.).

## Examples

```
/publish-report sprint-report "TaskFlow"
/publish-report retro "TaskFlow"
/publish-report release-notes "TaskFlow" --version v2.0
/publish-report roadmap "TaskFlow"
/publish-report milestone-update "TaskFlow" "v2.0 Public Beta"
```

## Best Practices

- Sprint reports: publish within 24 hours of sprint close
- Retros: publish immediately after the ceremony
- Release notes: two versions — internal (with all items) and customer-facing (curated)
- Roadmap: update weekly, do not recreate — edit the existing page

</details>

### `relate`

Create a relation between two Plane work items (block, duplicate, relates-to)

Arguments: `<relation> <project> <from-item> <to-item>`

<details><summary>Prompt template</summary>

# Relate

Create or remove a typed relation between two work items.

## Arguments

Format: `<relation> <project> <from-item> <to-item>`

- `relation`: `blocks` | `blocked-by` | `duplicate` | `duplicate-of` | `relates-to` | `remove`
- `project`: project name or identifier
- `from-item`: source work item identifier (`PROJ-42`)
- `to-item`: target work item identifier (`PROJ-43`)

Parse from `"$ARGUMENTS"`.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `create_work_item_relation`, `list_work_item_relations`, `remove_work_item_relation`.
2. **Resolve project** → `project_id`. **Resolve both work items** → UUIDs.
3. **Map relation type** to API value. Plane typically uses: `blocking` / `blocked_by` / `duplicate` / `relates_to`. Check schema and adapt. Note the API field is usually `issues: [uuid]` (plural array), not `related_issue`.
4. **Route**:
   - `blocks` → from `from-item` create `blocking` to `to-item` (API may auto-create the inverse `blocked_by` on the target)
   - `blocked-by` → from `from-item` create `blocked_by` to `to-item`
   - `duplicate` / `duplicate-of` → `duplicate` relation
   - `relates-to` → `relates_to`
   - `remove` → `list_work_item_relations`, find the matching one, call `remove_work_item_relation`
5. **Confirm** — print both items and the relation direction.

## Examples

```
/relate blocks "TaskFlow" PROJ-148 PROJ-150
/relate blocked-by "TaskFlow" PROJ-150 PROJ-148
/relate duplicate "TaskFlow" PROJ-160 PROJ-148
/relate relates-to "TaskFlow" PROJ-148 PROJ-200
/relate remove "TaskFlow" PROJ-148 PROJ-150
```

## Best Practices

- Set blockers **as soon as you discover them**, not in retro. Blockers visible early let the team unblock themselves.
- Mark duplicates with `duplicate-of`, then close the duplicate with a comment linking to the canonical item.
- Use `/dependencies` after blocking to see the full graph and detect chains/cycles.

</details>

### `retro`

Run a sprint retrospective and create action items

Arguments: `<project> [--format start-stop-continue|daki|4ls]`

<details><summary>Prompt template</summary>

# Retro

Run a sprint retrospective — present metrics, collect team feedback, create action items, save notes.

## Arguments
Format: `<project> [--format start-stop-continue|daki|4ls]`
- project: Project name or identifier (required)
- --format: Retro format (default: start-stop-continue)

Parse from "$ARGUMENTS".

## Process

0. **Bootstrap connector** — consult the `connector-bootstrap` skill. Probe `ToolSearch` for the action names referenced below (`list_projects`, `list_cycles`, etc.). Match tools by action suffix — never assume a specific MCP prefix. If multiple Plane instances are connected, ask the user which one to use. All formulas and rules come from the `agile-fundamentals` skill.

1. **Get sprint data:**
   ```
   list_cycles({ project_id })
   ```
   Find active or most recently completed cycle.
   ```
   list_cycle_work_items({ project_id, cycle_id })
   ```

2. **Generate sprint metrics:**
   - Completion rate (points and items)
   - Velocity vs historical average
   - Scope changes (items added/removed mid-sprint)
   - Blockers encountered

3. **Present retro template:**
   Based on --format:

   **start-stop-continue:**
   - What should we START doing?
   - What should we STOP doing?
   - What should we CONTINUE doing?

   **daki:**
   - DROP: What's not working?
   - ADD: What new practices to adopt?
   - KEEP: What's working well?
   - IMPROVE: What can be better?

   **4ls:**
   - LIKED: What did we enjoy?
   - LEARNED: What did we learn?
   - LACKED: What was missing?
   - LONGED FOR: What do we wish we had?

4. **Collect team input** (interactive conversation)

5. **Create action items (max 2-3):**
   ```
   create_work_item({
     project_id, name: "[RETRO] <action>",
     priority: "high", labels: ["retro-action-label-id"],
     assignees: ["<owner>"], target_date: "<next sprint end>"
   })
   ```

6. **Save retro notes:**
   ```
   create_project_page({
     project_id,
     name: "Retro — <sprint name> (<date>)",
     description_html: "<formatted retro notes>"
   })
   ```

## Example Usage
```
/retro "TaskFlow"
/retro "My Project" --format daki
/retro "ShopFlow" --format 4ls
```

</details>

### `roadmap`

Generate a Now / Next / Later roadmap view across cycles, modules, epics

Arguments: `<project> [--horizon <weeks>]`

<details><summary>Prompt template</summary>

# Roadmap

Produce a Now / Next / Later / Parked roadmap view for a project, aggregating active cycles, modules, epics, and milestones.

## Arguments

Format: `<project> [--horizon <weeks>]`

Parse from `"$ARGUMENTS"`. Default horizon: 12 weeks.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `list_cycles`, `list_modules`, `list_epics`, `list_milestones`.
2. **Resolve project** — `list_projects` → `project_id`.
3. **Gather horizon data**:
   - `list_cycles({ project_id })` — current and next sprint
   - `list_modules({ project_id })` — active modules
   - `list_epics({ project_id })` — active epics
   - `list_milestones({ project_id })` — upcoming milestones in the horizon
4. **Classify each item**:
   - **Now** — in the current sprint or module status `in-progress`
   - **Next** — in the next planned sprint or starting within 2 weeks
   - **Later** — within the horizon but not yet scheduled
   - **Parked** — explicitly deprioritized or blocked indefinitely
5. **Compute progress** for each module/epic — completion percentage, blockers, risk (see `epics-initiatives-milestones`).
6. **Render** as a markdown table grouped by horizon bucket, or offer to publish as a Plane page via `/publish-report roadmap`.

## Example

```
/roadmap "TaskFlow"
/roadmap "TaskFlow" --horizon 8
```

## Output Format

```
### Now
- **Billing v2** (module) — 65%, target Apr 30, 🟢
- **Login fixes** (cycle 14) — in progress

### Next
- **Multi-tenant support** (epic) — starts Sprint 16

### Later
- **Enterprise SSO** (epic) — scoping, target Q3
- **v2.0 Public Beta** (milestone) — Apr 30

### Parked
- **Mobile app** — revisit Q3
```

</details>

### `setup-project`

Set up a new Agile-ready project with recommended states, labels, and work item types

Arguments: `<project-name> [--template lean|standard]`

<details><summary>Prompt template</summary>

# Setup Project

Configure a new or existing Plane project with Agile-optimized states, labels, work item types, and features.

## Arguments
Format: `<project-name> [--template lean|standard]`
- project-name: Name of the project (required)
- --template: Configuration template (default: lean)
  - lean: Minimal setup for 1-3 person teams
  - standard: Full Agile setup for 4+ person teams

Parse from "$ARGUMENTS".

## Process

0. **Bootstrap connector** — consult the `connector-bootstrap` skill. Probe `ToolSearch` for the action names referenced below (`list_projects`, `list_cycles`, etc.). Match tools by action suffix — never assume a specific MCP prefix. If multiple Plane instances are connected, ask the user which one to use. All formulas and rules come from the `agile-fundamentals` skill.

1. **Create or find project:**
   ```
   list_projects()  // check if exists
   create_project({ name, identifier: "<auto-generated>" })  // if new
   ```

2. **Enable features:**
   ```
   update_project_features({
     project_id,
     cycles: true,    // Sprints
     pages: true,     // Retro notes
     modules: true,   // Feature grouping (standard only)
     epics: true,     // Epic tracking (standard only)
     views: true      // Custom views (standard only)
   })
   ```

3. **Create states:**
   Lean: Backlog → Todo → In Progress → Done
   Standard: Backlog → Todo → In Progress → In Review → Done → Cancelled

   ```
   create_state({ project_id, name, color, group, sequence })
   ```

4. **Create labels:**
   Type: bug, feature, tech-debt
   Status: ready, needs-refinement, blocked, retro-action
   Additional (standard): spike, chore, quick-win

   ```
   create_label({ project_id, name, color })
   ```

5. **Create work item types (standard only):**
   ```
   create_work_item_type({ project_id, name: "Story" })
   create_work_item_type({ project_id, name: "Task" })
   create_work_item_type({ project_id, name: "Bug" })
   create_work_item_type({ project_id, name: "Spike" })
   ```

6. **Verify setup:**
   ```
   list_states({ project_id })
   list_labels({ project_id })
   get_project_features({ project_id })
   ```
   Confirm everything is configured.

## Example Usage
```
/setup-project "My Startup App"
/setup-project "Enterprise Platform" --template standard
```

</details>

### `sprint-status`

Show current sprint status — progress, burndown, at-risk items

Arguments: `<project>`

<details><summary>Prompt template</summary>

# Sprint Status

Display a dashboard of the current sprint — progress, burndown, WIP, and at-risk items.

## Arguments
Format: `<project>`
- project: Project name or identifier (required)

Parse from "$ARGUMENTS".

## Process

0. **Bootstrap connector** — consult the `connector-bootstrap` skill. Probe `ToolSearch` for the action names referenced below (`list_projects`, `list_cycles`, etc.). Match tools by action suffix — never assume a specific MCP prefix. If multiple Plane instances are connected, ask the user which one to use. All formulas and rules come from the `agile-fundamentals` skill.

1. **Find active sprint:**
   ```
   list_cycles({ project_id })
   ```
   Find cycle where today is between start_date and end_date.

2. **Get sprint details:**
   ```
   retrieve_cycle({ project_id, cycle_id })
   list_cycle_work_items({ project_id, cycle_id })
   ```

3. **Calculate burndown:**
   - Total points, completed points, remaining points
   - Sprint days total, days elapsed, days remaining
   - Ideal remaining = total × (remaining_days / total_days)
   - Status: ON TRACK / AT RISK / BEHIND

4. **Check WIP:**
   ```
   get_project_members({ project_id })
   ```
   WIP limit = team_size × 1.5
   Current WIP = items in "started" state

5. **Identify at-risk items:**
   - Items in "started" state for > 2 days
   - Items not started past mid-sprint
   - Items with "blocked_by" relations

6. **Display dashboard:**
   Sprint name, dates, progress bar, burndown status, WIP, at-risk items, per-person summary.

## Example Usage
```
/sprint-status "TaskFlow"
/sprint-status "My Project"
```

</details>

### `standup`

Generate daily standup summary for current sprint

Arguments: `<project>`

<details><summary>Prompt template</summary>

# Standup

Generate a daily standup summary — per-person status, sprint progress, and blockers.

## Arguments
Format: `<project>`
- project: Project name or identifier (required)

Parse from "$ARGUMENTS".

## Process

0. **Bootstrap connector** — consult the `connector-bootstrap` skill. Probe `ToolSearch` for the action names referenced below (`list_projects`, `list_cycles`, etc.). Match tools by action suffix — never assume a specific MCP prefix. If multiple Plane instances are connected, ask the user which one to use. All formulas and rules come from the `agile-fundamentals` skill.

1. **Find active sprint:**
   ```
   list_cycles({ project_id })
   ```
   Find cycle where today is between start_date and end_date.

2. **Get sprint items and team:**
   ```
   list_cycle_work_items({ project_id, cycle_id })
   get_project_members({ project_id })
   ```

3. **Group items by assignee and state:**
   For each team member:
   - Completed: items in "completed" state group
   - In Progress: items in "started" state group
   - Not Started: items in "unstarted" state group

4. **Detect blockers:**
   For in-progress items, check:
   ```
   list_work_item_relations({ project_id, work_item_id })
   ```
   Flag items with "blocked_by" relations.

5. **Calculate sprint progress:**
   - Total/completed/remaining points
   - Days elapsed and remaining
   - Pace vs needed pace
   - WIP count vs limit

6. **Display standup report:**
   Per-person summary (done, doing, blocked) + team-level sprint progress.

## Example Usage
```
/standup "TaskFlow"
/standup "My Project"
```

</details>

### `state`

Manage Plane workflow states — list, create, update, delete, reorder

Arguments: `<action> <project> [args...]`

<details><summary>Prompt template</summary>

# State

Manage workflow states (the columns on a board) for a Plane project.

## Arguments

Format: `<action> <project> [args...]`

- `action`: `list` | `create` | `update` | `delete`
- `project`: project name or identifier
- For `create`/`update`: `--name`, `--group <backlog|unstarted|started|completed|cancelled>`, `--color #hex`, `--sequence <int>`, `--default`

Parse from `"$ARGUMENTS"`.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `list_states`, `create_state`, `update_state`, `delete_state`, `retrieve_state`.
2. **Resolve project** → `project_id`.
3. **Route**:
   - `list` → `list_states({ project_id })`. Render grouped by `group`, sorted by `sequence`. Mark the default state.
   - `create` → require `--name` and `--group`. Color defaults from a stable palette per group. Call `create_state`.
   - `update` → resolve state by name → `update_state`. Renaming a state does NOT migrate items — they keep the same `state_id`.
   - `delete` → confirm. **Block deletion** if any items currently sit in this state — instruct the user to migrate them first via `/bulk-update --state <new>`.
4. **Confirm** — re-list states after mutation.

## State group semantics

Plane groups are fixed: `backlog`, `unstarted`, `started`, `completed`, `cancelled`. Velocity counts only items moved into a `completed` group state during the cycle. Reports separate `cancelled` from `completed`. **Don't put "Done" in the `started` group** — it breaks burndown.

## Recommended state set (Scrum)

| Group | States |
|---|---|
| backlog | Backlog |
| unstarted | Ready, To Do |
| started | In Progress, In Review |
| completed | Done |
| cancelled | Cancelled, Won't Fix |

See `labels-states-properties` skill for variant sets (Kanban, support, research).

## Examples

```
/state list "TaskFlow"
/state create "TaskFlow" --name "In Review" --group started --color #f59e0b
/state update "TaskFlow" "In Review" --sequence 35
/state delete "TaskFlow" "Old Column"
```

</details>

### `triage-intake`

Triage the Plane intake inbox — accept, defer, or reject incoming items

Arguments: `<project> [--limit <n>]`

<details><summary>Prompt template</summary>

# Triage Intake

Run a triage session over the Plane intake queue for a project.

## Arguments

Format: `<project> [--limit <n>]`

Parse from `"$ARGUMENTS"`. Default limit is 20 items.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `list_intake_work_items`, `retrieve_intake_work_item`, `create_work_item`.
2. **Resolve project** — `list_projects` → `project_id`.
3. **Load intake queue** — `list_intake_work_items({ project_id })`. Sort by age (oldest first).
4. **Triage each item** — follow the `intake-triage` skill. For each item ask the user (or classify automatically using the routing rules):
   - **Accept** → convert to work item in the backlog
   - **Accept + escalate** → create work item, set urgent/high, assign on-call
   - **Defer** → mark with rationale and revisit date
   - **Reject** → delete with a comment
5. **Present session summary** — table of decisions made.
6. **Suggest follow-ups** — items needing more info from the reporter, duplicates to close, deferred items to revisit next grooming.

## Example

```
/triage-intake "TaskFlow"
/triage-intake "TaskFlow" --limit 10
```

## Output Format

```
| # | Item | Age | Decision | Result |
|---|------|-----|----------|--------|
| 1 | "Login 500 Safari" | 2d | Accept+escalate | PROJ-148, urgent, @alice |
| 2 | "Dark mode request" | 5d | Defer | Revisit in 2 sprints |
| 3 | "Same login bug" | 1d | Reject | Duplicate of PROJ-148 |
```

See the `intake-triage` skill for routing rules and cadence recommendations.

</details>

### `velocity`

Calculate team velocity from historical sprint data

Arguments: `<project> [--sprints <count>]`

<details><summary>Prompt template</summary>

# Velocity

Calculate team velocity from completed sprints and show trend analysis.

## Arguments
Format: `<project> [--sprints <count>]`
- project: Project name or identifier (required)
- --sprints: Number of past sprints to analyze (default: 5)

Parse from "$ARGUMENTS".

## Process

0. **Bootstrap connector** — consult the `connector-bootstrap` skill. Probe `ToolSearch` for the action names referenced below (`list_projects`, `list_cycles`, etc.). Match tools by action suffix — never assume a specific MCP prefix. If multiple Plane instances are connected, ask the user which one to use. All formulas and rules come from the `agile-fundamentals` skill.

1. **Resolve project:**
   ```
   list_projects()
   ```

2. **Get completed sprints:**
   ```
   list_archived_cycles({ project_id })
   ```
   Take last N archived cycles (sorted by end_date descending).

3. **Calculate velocity per sprint:**
   For each cycle:
   ```
   list_cycle_work_items({ project_id, cycle_id })
   ```
   Sum points of items in "completed" state group.
   Also sum total planned points.

4. **Calculate aggregates:**
   - Average velocity
   - Min/max range
   - Average completion rate
   - Trend (improving/stable/declining)

5. **Display velocity report:**
   ```
   | Sprint | Planned | Completed | Rate |
   |--------|---------|-----------|------|
   | Sprint 11 | 40 | 36 | 90% |
   | Sprint 10 | 38 | 32 | 84% |
   | ...

   Average velocity: 33 pts/sprint
   Range: 28-36 pts
   Completion rate: 86%
   Trend: Stable
   Recommended next sprint: ~28 pts (velocity × 0.85 buffer)
   ```

## Example Usage
```
/velocity "TaskFlow"
/velocity "My Project" --sprints 10
```

</details>

### `whoami`

Show the current Plane user, workspace, and connector status

Arguments: `(no arguments)`

<details><summary>Prompt template</summary>

# Who Am I

Print the identity and context Plane is currently operating with. Useful for verifying the right account/instance is connected before destructive operations.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `get_me`, `get_workspace_features`, `list_projects`.
2. **Identity** — `get_me`. Print: display name, email, user_id, role, avatar URL.
3. **Workspace** — workspace name and slug from `get_me` response or `get_workspace_features`. Print enabled features.
4. **Projects** — `list_projects`, count by archived/active. Print top 5 by recent activity.
5. **Connector** — print which MCP server / instance the tools were resolved against (from `connector-bootstrap` result). If multiple Plane instances are connected, list them and indicate the active one.
6. **Suggest** — if no projects, suggest `/setup-project`. If multiple instances, show how to switch.

## Examples

```
/whoami
```

## Why this exists

Before running anything destructive (`/cycles delete`, `/projects delete`, `/state delete`), run `/whoami` to make sure you are not accidentally connected to a production workspace when you meant the staging one. Cheap insurance.

</details>

### `work-item-type`

Manage custom work item types in a Plane project (Story, Bug, Spike, ...)

Arguments: `<action> <project> [args...]`

<details><summary>Prompt template</summary>

# Work Item Type

Define and manage custom work item types — beyond the default Issue/Task. Examples: Story, Bug, Spike, Tech Debt, Incident.

## Arguments

Format: `<action> <project> [args...]`

- `action`: `list` | `create` | `update` | `delete` | `get`
- `project`: project name or identifier
- For `create`/`update`: `--name`, `--description`, `--icon`, `--color`, `--default`, `--enabled`

Parse from `"$ARGUMENTS"`.

## Process

1. **Bootstrap connector** — consult `connector-bootstrap`. Probe for `list_work_item_types`, `create_work_item_type`, `update_work_item_type`, `delete_work_item_type`, `retrieve_work_item_type`. Note: this feature requires a Plane build with custom work item types enabled in workspace features.
2. **Resolve project** → `project_id`.
3. **Route**:
   - `list` → render: name | icon | default? | usage count
   - `create` → call `create_work_item_type`. Suggest pairing with custom properties (`/property`).
   - `update` → resolve by name → patch.
   - `delete` → confirm. Items of this type need migration first; warn if any exist.
   - `get` → details + property schema attached to the type.
4. **Confirm** — re-list after mutation.

## Recommended type sets

**Standard product team:** Story, Bug, Task, Spike, Tech Debt
**Platform team:** Feature, Bug, Incident, Runbook, RFC
**Support team:** Ticket, Bug, Question, Escalation

Don't go beyond 6 types — taxonomy fatigue is real. See `labels-states-properties` skill for guidance on type vs label vs property.

## Examples

```
/work-item-type list "TaskFlow"
/work-item-type create "TaskFlow" --name Spike --icon flask --color #8b5cf6 --description "Time-boxed research"
/work-item-type update "TaskFlow" Story --default
/work-item-type delete "TaskFlow" "Old Type"
```

</details>

### `work-item`

Create, update, or inspect a work item (issue/task/ticket) in Plane

Arguments: `<action> <project> [args...]`

<details><summary>Prompt template</summary>

# Work Item

Create, update, or inspect a single work item in Plane. Delegates all tool resolution to the `connector-bootstrap` skill and all field rules to the `work-items` skill.

## Arguments

Format: `<action> <project> [args...]`

- `action`: one of `create`, `update`, `get`, `search`, `comment`, `link`, `log-time`, `block`
- `project`: project name or identifier
- Remaining args depend on action (see examples)

Parse from `"$ARGUMENTS"`.

## Process

1. **Bootstrap connector** — consult the `connector-bootstrap` skill. Probe `ToolSearch` for work item actions (`create_work_item`, `update_work_item`, `search_work_items`, `retrieve_work_item_by_identifier`) before assuming anything. If multiple Plane instances are connected, ask the user which one.

2. **Resolve project** — `list_projects` → pick `project_id` by name or identifier.

3. **Route by action** — follow the matching section of the `work-items` skill:
   - `create` → gather title, description with AC, priority, points, assignees, labels; call `create_work_item` (check schema for `state`/`state_id` and `labels`/`label_ids` field names)
   - `update` → resolve work item ID, apply field changes via `update_work_item`
   - `get` → when the user passes a human identifier like `PROJ-42`, split into project slug `PROJ` and integer `42`, then call `retrieve_work_item_by_identifier({ project_identifier, issue_identifier })`. For UUIDs, use `retrieve_work_item({ project_id, work_item_id })`.
   - `search` → `search_work_items({ query })` — typically workspace-scoped, filter by project client-side if needed
   - `comment` → `create_work_item_comment`
   - `link` → `create_work_item_link` (PRs, docs, designs)
   - `log-time` → `create_work_log` with duration as **integer minutes** (convert "2h 30m" → 150); project must have `is_time_tracking_enabled`
   - `block` → `create_work_item_relation` with `relation_type: "blocked_by"` and `issues: [uuid]` (array, plural — not `related_issue`)

4. **Validate** — before creating or committing to a sprint, verify the Definition of Ready from the `agile-fundamentals` skill.

5. **Confirm** — print a short summary of what was created/changed and the item identifier.

## Examples

```
/work-item create "TaskFlow" "Fix login 500 on Safari" --priority high --points 3 --labels bug
/work-item update "TaskFlow" PROJ-148 --state "In Progress" --assignee alice
/work-item get "TaskFlow" PROJ-148
/work-item search "TaskFlow" "login bug"
/work-item comment "TaskFlow" PROJ-148 "Reproduced on Safari 17, investigating"
/work-item link "TaskFlow" PROJ-148 https://github.com/org/repo/pull/420 "PR #420"
/work-item log-time "TaskFlow" PROJ-148 PT2H30M "Debug + fix"
/work-item block "TaskFlow" PROJ-148 PROJ-140
```

</details>

### `wsjf-prioritize`

Prioritize backlog items using Weighted Shortest Job First (WSJF) scoring

Arguments: `<project>`

<details><summary>Prompt template</summary>

# WSJF Prioritize

Prioritize backlog items using Weighted Shortest Job First scoring — balancing value, urgency, risk, and effort.

## Arguments
Format: `<project>`
- project: Project name or identifier (required)

Parse from "$ARGUMENTS".

## Process

0. **Bootstrap connector** — consult the `connector-bootstrap` skill. Probe `ToolSearch` for the action names referenced below (`list_projects`, `list_cycles`, etc.). Match tools by action suffix — never assume a specific MCP prefix. If multiple Plane instances are connected, ask the user which one to use. All formulas and rules come from the `agile-fundamentals` skill.

1. **Resolve project:**
   ```
   list_projects()
   ```

2. **Get estimated backlog items:**
   ```
   list_work_items({ project_id })
   ```
   Filter: items in backlog/unstarted states WITH story points set.
   (Items without estimates cannot be scored — suggest /estimate first.)

3. **Score each item interactively:**
   For each item, present and ask:
   - **Business Value** (1-10): How much user/business value does this deliver?
   - **Time Criticality** (1-10): How urgent is this? Is there a deadline?
   - **Risk Reduction** (1-10): Does this reduce technical or business risk?
   - **Job Size**: Use existing story points

4. **Calculate WSJF:**
   ```
   WSJF = (Business Value + Time Criticality + Risk Reduction) / Story Points
   ```
   Higher WSJF = higher priority.

5. **Sort and assign priorities:**
   ```
   Top 25%    → priority: "urgent"
   25-50%     → priority: "high"
   50-75%     → priority: "medium"
   Bottom 25% → priority: "low"
   ```

6. **Update items:**
   ```
   update_work_item({ project_id, work_item_id, priority: "<new>" })
   ```

7. **Display ranked list:**
   ```
   | Rank | Item | BV | TC | RR | Size | WSJF | Priority |
   |------|------|----|----|-----|------|------|----------|
   | 1 | Quick fix | 8 | 9 | 5 | 2 | 11.0 | urgent |
   | 2 | Key feature | 9 | 7 | 6 | 5 | 4.4 | high |
   | ... |
   ```

## Example Usage
```
/wsjf-prioritize "TaskFlow"
/wsjf-prioritize "My Project"
```

</details>
