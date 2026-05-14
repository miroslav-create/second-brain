# Handover — second-brain workflow

> For future Claude Code sessions picking up this system. Read this if user says "let's tweak the workflow" or "something's off with X".

## TL;DR

Personal workflow integrating Notion (task hub) + Obsidian (PKM) + Claude Code (partner). Built 2026-05-13 across three sessions: (1) initial design via `/grill-with-docs`, (2) view cleanup + Notion UI configuration, (3) critical review + DB migration + instrumentation.

The system is **live and instrumented**. Notion DB migrated to plain schema (escaped typed-Tasks-template lock); Project property added; daily template carries an inbox-count thermometer (threshold 15 7-day avg → add Friday skim ritual); SKILL.md hand-offs documented; original design plan archived to `docs/design-2026-05-13.md`. **One UI step remains** (rename old DB in Notion as backup) and one minor housekeeping (rename `_unassigned_` Project option when first real project lands).

Design plan audit trail at both `~\.claude\plans\goal-of-this-session-glimmering-spark.md` (origin) and `docs/design-2026-05-13.md` (in-repo frozen copy). Review session output at `~\.claude\plans\let-s-review-everything-start-declarative-nest.md`.

## How to operate

For the user-facing daily/weekly use: [`HOW-TO-USE.md`](HOW-TO-USE.md).
For the user/CC operating contract in this dir: [`CLAUDE.md`](CLAUDE.md).
For the live glossary including all IDs: [`CONTEXT.md`](CONTEXT.md).

## Current state (live as of 2026-05-13 end-of-day)

### What's live
- **Filesystem scaffold** under `c:\Users\miroslav.zachar\OneDrive - Direct\Projects\second-brain\` (OneDrive-synced).
- **Git**: local repo on branch `main`, tracks `origin/main` at `https://github.com/miroslav-create/second-brain.git`. Hourly auto-commit + push via Obsidian Git plugin operational.
- **Obsidian vault** at `vault/`. Templater + Obsidian Git installed and configured. Daily Notes core plugin → template `_templates/daily.md`. Old Fondee vault (79 files) archived read-only at `vault/_archive/Fondee/`, excluded from graph view.
- **Notion MCP** authenticated as `miroslav.zachar@direct.cz`. Tools verified: fetch, search, query-data-sources, create-database, update-data-source, create-view, update-view, create-pages, update-page.
- **Notion private Kanban DB**: title `Second Brain`, ID `4f21d60dc0494d6f84e3248708e87af3`, data source ID `c0a6b990-bdb5-42e3-b112-6c8d424f3819`. Schema (post-migration to plain DB):
  - `Task name` (title)
  - `Status` (**select**) — options: `Inbox | Today | Doing | Waiting | Done | Archive`. Fully API-mutable.
  - `Type` (select) — `delivery | code | learn | admin | people | decision`
  - `Due` (date)
  - `Project` (select) — currently 1 placeholder option `_unassigned_`. Slugs to mirror `vault/projects/<slug>.md` filenames as projects are created.
  - **No Assignee** (escaped typed-Tasks-template lock).
- **Skills** (`.claude/skills/`): `/inbox`, `/spar`, `/draft`. Each has explicit hand-off lines pointing at the other two skills when appropriate. `/inbox` was renamed from `/triage` to avoid collision with Matt Pocock issue-tracker skill at parent `Projects\.claude\skills\triage`.
- **Daily template** (`vault/_templates/daily.md`) contains `**Notion Inbox count:** ___` line under `## Inbox`. User fills the count each day; threshold 15 7-day rolling avg → confirms on-demand-discipline failure → add Friday skim ritual.
- **Settings**: project-level `.claude/settings.json` committed (Notion read+write tools, read-only git). Destructive perms (`git push`, `git commit`) live only in `.claude/settings.local.json` (gitignored).
- **Auto-memory** at `~\.claude\projects\c--Users-miroslav-zachar-OneDrive---Direct-Projects-second-brain\memory\` (path may show `-scaffolding` from an earlier rename — check both).

### Open / pending user actions
1. **Notion UI — rename old DB to `[archived] Second Brain v1`**. URL: https://www.notion.so/35f18e00108d80c3985bcecb788758e2 — keep as backup for 30 days, then delete. (API can't rename typed-Tasks-template DB title directly + leaves safety window for any forgotten data.)
2. **Notion UI — rename `_unassigned_` Project option** to whatever first real project slug is when you create the first `vault/projects/<slug>.md`. Or delete it later via DDL when at least one real option exists.

### Open question for next session — morning flow too heavy
User asked for a model of morning + workday flow on 2026-05-13 evening. Three iterations were rejected as too many pre-work steps. The current state of the discussion:

- **User's complaint**: before starting actual work, need 3+ steps (open CC, open Obsidian, write thermometer, do check-in). Too heavy. Wants: sit down → start working. Period.
- **Constraint the design must satisfy**: thermometer (or some falsifiability signal) survives per the [review session resolution](C:\Users\miroslav.zachar\.claude\plans\let-s-review-everything-start-declarative-nest.md) — threshold 15 7-day avg triggers Friday-skim ritual. Drop only if user explicitly accepts losing the experiment.
- **Three cuts proposed (not yet picked)**:
  1. **Zero-ceremony auto** — CC runs morning check on first message of day, writes to `vault/thermometer.md` (new file, NOT daily note), prepends status to response. Thermometer moves out of daily note → single CC-managed running log. Requires a rule in CLAUDE.md.
  2. **Drop thermometer entirely** — bet on raw discipline + visual Notion Inbox count as canary. Cheapest. Loses falsifiability.
  3. **Auto-thermometer in daily note** — CC writes thermometer line into daily note when present; otherwise prompts user to open Obsidian. 1-2 steps depending on day.
- **Files that would change if option (1) picked**: `vault/_templates/daily.md` (remove thermometer line), `vault/thermometer.md` (new file, CC seeds), `CLAUDE.md` (add `on first response of session, check thermometer last entry; if not today, run morning routine` rule), `HOW-TO-USE.md` (update Daily flow + thermometer section).
- **Files that would change if option (2) picked**: `vault/_templates/daily.md` (remove thermometer line), `HOW-TO-USE.md` (drop thermometer section), `HANDOVER.md` + review plan file (note thermometer dropped, weak point #1 becomes unobserved).
- **Files that would change if option (3) picked**: `CLAUDE.md` (auto-write rule, fallback to prompt). Smallest change.
- **What to do next session**: surface this open question first, ask user to pick the cut, then implement.

### Live views on private Kanban (NEW DB `4f21d60dc0494d6f84e3248708e87af3`)
| Name | Type | View ID |
|--|--|--|
| Board by Status | board, GROUP BY Status | `35f18e00-108d-81fc-8cfa-000cb2d28097` |
| Today | board, GROUP BY Status, filter Status=Today | `35f18e00-108d-81bc-9f1e-000c3910dce3` |
| Inbox | board, GROUP BY Status, filter Status=Inbox | `35f18e00-108d-81ec-8657-000c3181a89d` |
| By Type | board, GROUP BY Type | `35f18e00-108d-817b-9c4a-000c0ed05add` |
| By Project | board, GROUP BY Project | `35f18e00-108d-8142-a73e-000cdcef4d21` |

Schema: `Task name`, `Status` (select), `Type` (select), `Due` (date), `Project` (select). No Assignee. Status options: `Inbox | Today | Doing | Waiting | Done | Archive`. Status filters bind correctly via MCP DSL now (was the status-property-type bug).

### Old DB (archived) — `35f18e00108d80c3985bcecb788758e2`
Pre-migration views on the old DB are deprecated. Old DB will be archived (renamed in UI) then deleted after 30-day safety window.

### Resolved 2026-05-13 (second session)
- Initial git push done. Branch `main` tracks `origin/main`. Hourly auto-push via Obsidian Git operational.
- CLAUDE.md slimmed to <150 words. Team-board IDs hoisted to CONTEXT.md.
- 4 duplicate views from MCP create_view round deleted by user; pre-existing boards restored to board type after accidental table-conversion during cleanup.
- `Archive` status option added in UI. `Inbox` view filter fixed (was bugged to Status=Today since initial setup).
- Detected 4 pre-existing views on DB (created in earlier setup) — first design plan claimed views needed to be created. **Procedure now: always `notion-fetch <db_id>` before creating views.**
- View type (board ↔ table) not settable via MCP `notion-update-view` — only via Notion UI Layout setting. Codified in Known limitations.
- Old vault retired: `Dokumenty\Obsidian` removed from Obsidian's vault list.

### Resolved 2026-05-13 (third session — review + migration)
- Critical review of system design produced as `~/.claude/plans/let-s-review-everything-start-declarative-nest.md`. Four weak points drilled: (1) on-demand discipline → thermometer instrument, (2) two-inbox friction → resolved via design clarification (CC conversation = primary inbox), (4) typed-collection tax → migrate now, (8/9) Project property added, Hat rejected with empirical reasoning.
- **Notion DB migrated** from typed-Tasks-template (`35f18e00108d80c3985bcecb788758e2`) to plain DB (`4f21d60dc0494d6f84e3248708e87af3`, data source `c0a6b990-bdb5-42e3-b112-6c8d424f3819`). Status is now `select` type, fully API-mutable. 0 rows to migrate.
- 5 views recreated on new DB. Status filters bind correctly via MCP DSL (was status-property-type bug).
- Project property added (`select`, with `_unassigned_` placeholder to be renamed at first real project birth).
- Thermometer line added to `vault/_templates/daily.md`: `**Notion Inbox count:** ___` (threshold 15 7-day avg → trip → add Friday skim ritual).
- Hand-off lines added to `inbox/spar/draft` SKILL.md (explicit `if user wants X, prefer skill Y` guidance).
- Project-level `.claude/settings.json` created with safe baseline (Notion read+write tools, read-only git). Destructive perms stay in `settings.local.json`.
- Original `/grill-with-docs` design plan archived to `docs/design-2026-05-13.md` in repo (was machine-local at `~/.claude/plans/`).

### Known limitations (durable — codify, don't keep re-discovering)
- **Notion typed-collection lock**: Tasks-template DBs in Notion mark certain properties as "required". The MCP cannot:
  - drop `Assignee` (typed property)
  - change `Status` from `status` type to anything else
  - mutate `status` property options via DDL (use Notion UI) — re-confirmed 2026-05-13 by `ALTER COLUMN "Status" SET STATUS(...)` → 400 validation_error
- **Notion MCP DSL — status filter gap**: `FILTER "<status-property>" = "<option>"` and `IN ("<option>")` both silently produce empty `advancedFilter.filters` arrays on view create/update. Same DSL on `select` properties binds correctly. Workaround: filter status views in Notion UI. Re-test on future MCP versions.
- **Notion MCP DSL — `simpleFilters` legacy schema not writable**: views created in Notion UI may carry filters under `simpleFilters` (legacy schema) instead of `advancedFilter`. MCP `notion-update-view` DSL writes only to `advancedFilter`, leaving any `simpleFilters` intact. Net effect: can't fix or remove legacy filters via API. UI required.
- **No view delete via MCP**: `notion-update-view` has no trash flag and no `delete-view` tool exists. Duplicate / unwanted views must be deleted via Notion UI (right-click tab → Delete view). Mitigation: rename via `notion-update-view` with `[dup-delete]` prefix so user can spot them.
- **No view-type setter via MCP**: `notion-update-view` accepts `name` + `configure` only — no way to flip a table view to a board (or vice-versa). UI-only via Layout selector.
- **Notion MCP archive/trash gap**: `notion-update-page` / equivalent does not expose page archive or trash. To "discard" a row: set `Status=Done` and prepend `[discarded] ` to the title. True trash = manual user action in Notion UI. Codified in `.claude/skills/inbox/SKILL.md`.
- **Skill name collision**: parent `Projects\.claude\skills\triage` is Matt Pocock's issue-tracker skill. Shadows anything called `/triage` when CC is launched outside `second-brain/`. Our workflow skill is named `/inbox` to avoid this. Always launch CC from inside this dir for skill visibility.
- **No voice / transcription**: explicitly rejected by user during design. Mobile capture is type-only.
- **No automation / scheduling**: user explicitly rejected fixed-time rituals (no morning triage, no weekly review). Do not propose cron, ScheduleWakeup, or recurring routines. See memory file `feedback_no_rituals.md`.

## Design decisions and rationale

These were resolved during the `/grill-with-docs` session. If a future change feels like it touches one of these, it might be reopening a decision — be explicit about that with the user.

| Decision | Choice | Why |
|--|--|--|
| Working dir | `Projects\second-brain\` with vault as subfolder | Clean separation of config from vault. Easy git/backup. |
| Vault sync | OneDrive (existing) + private GitHub via Obsidian Git plugin | Durable + diffable + zero-cost. Rejected paid Obsidian Sync. |
| Capture entry | Mixed by context (Notion=work, Obsidian=thinking, mobile=typed-only) | Lower friction than single-target. Reconciled via `/inbox`. |
| Reconcile cadence | On-demand only | User rejected morning/weekly fixed rituals. |
| Kanban schema | Status / Type / Due / Project (post-migration). Hat explicitly rejected (swap multiple times per day). | Project added during 2026-05-13 migration. Hat reasoning: task-level Hat tag happens after execution, inverts the value; Hat belongs at project-level frontmatter if needed. Energy / Priority still deferred. |
| Obsidian roles ranked | Daily > Evergreen > Graph > Canvas | Graph view = "mind map", emergent from linking. Canvas reserved for diagrams. |
| Overlap rule (Notion vs Obsidian) | State machine → Notion. Prose+links → Obsidian. | Sharp rule. Use it when "which tool?" is unclear. |
| Skill scope for CC | Triage + sparring + drafting, user owns final output | Balanced leverage without trust burden. |
| Old vault | Read-only archive at `vault/_archive/` | Safety net for accidental gold; excluded from graph. |
| Plugin baseline | Obsidian Git + Templater + core Daily Notes/Templates | Dataview deferred. Adding more later is cheap; ripping out is hard. |

## How to extend or modify

### Add a new skill
Create `.claude/skills/<name>/SKILL.md` with frontmatter `name:` and `description:`. The description must explicitly say when to invoke (e.g., "Invoke when user types /foo"). Body is the skill's logic. Skill is automatically picked up by CC launched from this dir.

### Add Kanban property
Use `mcp__claude_ai_Notion__notion-update-data-source` with `ADD COLUMN`. Example:
```
ADD COLUMN "Priority" SELECT('P0':red, 'P1':orange, 'P2':yellow, 'P3':gray)
```
Then update `CONTEXT.md` schema section. Update `/inbox` SKILL.md if the new property should be set during triage.

### Add a new vault folder type
Update `CLAUDE.md` layout table. Add a Templater template in `_templates/`. Reference in `HOW-TO-USE.md` if user-facing. Folder name starting with `_` is excluded from graph view by convention (use for non-content folders like `_archive`, `_templates`).

### Change Notion DB
- Schema changes via `mcp__claude_ai_Notion__notion-update-data-source` (DDL).
- View create/update via `notion-create-view` / `notion-update-view` works for grouping, sorting, display props, and filters on select/text/date. Current DB uses `select` for Status — filters bind correctly via DSL. (Was a status-property-type bug on the old typed-Tasks-template DB; migrated 2026-05-13 to escape.)
- Status options on plain `select` Status property are fully mutable via DDL `ADD/DROP/ALTER COLUMN`.

### Re-migrate to another fresh Notion DB (if ever needed again)
The current plain DB was created 2026-05-13 to escape Notion's Tasks-template typed-collection lock. If you ever need to re-migrate (e.g., to add a different parent page, switch to a different workspace, or escape some future Notion API constraint):
1. Create new DB via `mcp__claude_ai_Notion__notion-create-database` — pass schema DDL, no parent for workspace-level placement (or specify a parent page ID).
2. Migrate active rows via `notion-create-pages` reading from old DB (use `notion-query-data-sources` with SQL).
3. Update DB ID + data source ID in `CONTEXT.md`. No skill IDs to update (skills lookup via CONTEXT.md).
4. Recreate views via `notion-create-view`. Verify status filters bind by fetching the new DB after creation.
5. Rename old DB to `[archived] <name>` in Notion UI; delete after 30-day safety window.

### Add a second Claude Code working dir (e.g., for a vibe-coding project)
Vibe-coding projects live as **sibling dirs** to `second-brain/` (e.g., `Projects\some-experiment\`), not inside it. Launch CC from inside the project dir. They will not see this dir's skills/CLAUDE.md unless you explicitly link.

## Files to be aware of

| File | Purpose | Edit when |
|--|--|--|
| `CLAUDE.md` | System instructions Claude reads on launch (~150 words) | You change persona, hard rules, or pointers to other files |
| `CONTEXT.md` | Live glossary including all DB IDs, schema, conventions | Any new entity, role, term, or IDs change |
| `HOW-TO-USE.md` | User-facing operator manual | Daily workflow changes, new skills, new rules |
| `HANDOVER.md` (this file) | State snapshot for future CC sessions | After major changes, before context risk |
| `docs/design-*.md` | Frozen historical design plans (do not edit) | Never — these are audit-trail records |
| `.claude/settings.json` | Project-level perms baseline (committed) | New tool needs perm at project level |
| `.claude/settings.local.json` | Machine-local perms (gitignored) | New tool needs perm for this machine only |
| `.claude/skills/<name>/SKILL.md` | Skill behavior | Behavior of that skill changes |
| `.gitignore` | Untracked paths | New machine-local files appear (settings.local.json, caches) |
| `vault/_templates/*.md` | Templater note templates | Note shape changes |

## Where the design conversation lives

The original `/grill-with-docs` session that produced this system:
- **In-repo frozen copy** (canonical): [`docs/design-2026-05-13.md`](docs/design-2026-05-13.md)
- **Original machine-local**: `c:\Users\miroslav.zachar\.claude\plans\goal-of-this-session-glimmering-spark.md` (may evaporate on `~/.claude` wipe)

The follow-up review session that produced the migration + thermometer + Project property:
- `c:\Users\miroslav.zachar\.claude\plans\let-s-review-everything-start-declarative-nest.md` — drill outcomes for weak points #1, #2, #4, #8/9.

Memory (auto-saved):
- `c:\Users\miroslav.zachar\.claude\projects\c--Users-miroslav-zachar-OneDrive---Direct-Projects-second-brain\memory\` (primary)
- `c:\Users\miroslav.zachar\.claude\projects\c--Users-miroslav-zachar-OneDrive---Direct-Projects-scaffolding\memory\` (legacy path from earlier rename — may or may not exist)

If the user wants to re-grill on a part of the design (e.g., "let's reconsider Hat rejection"), the appropriate plan file is the audit trail of what was decided and rejected. Surface the relevant one before re-debating.

## Caveman mode note

The user runs with `caveman` mode often. SKILL.md files specify caveman as default for process talk. The draft output of `/draft` is normal prose (caveman doesn't bleed into deliverables). This is intentional — keep it that way.
