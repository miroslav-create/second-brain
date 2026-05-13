# Handover — second-brain workflow

> For future Claude Code sessions picking up this system. Read this if user says "let's tweak the workflow" or "something's off with X".

## TL;DR

Personal workflow integrating Notion (task hub) + Obsidian (PKM) + Claude Code (partner). Built 2026-05-13 in a single design session using `/grill-with-docs`. The full design plan is at `~\.claude\plans\goal-of-this-session-glimmering-spark.md`. All decisions and rationale traced there.

The system is **live, not done**. Two manual user steps remain (see "Open / pending" below) and several second-order tweaks are deferred.

## How to operate

For the user-facing daily/weekly use: [`HOW-TO-USE.md`](HOW-TO-USE.md).
For the user/CC operating contract in this dir: [`CLAUDE.md`](CLAUDE.md).
For the live glossary including all IDs: [`CONTEXT.md`](CONTEXT.md).

## System state (snapshot 2026-05-13)

### What works
- Filesystem scaffold under `c:\Users\miroslav.zachar\OneDrive - Direct\Projects\second-brain\` (OneDrive-synced).
- Local git repo on branch `main`. Multiple commits. Remote `origin` set to `https://github.com/miroslav-create/second-brain.git`.
- Obsidian vault opened at `vault/`. Templater + Obsidian Git installed and configured. Auto-commit verified working (hourly + on edit).
- Templater wired to `_templates/` folder. Daily Notes core plugin points at `vault/daily/` with template `_templates/daily`.
- Old vault (79 files) archived to `vault/_archive/Fondee/`. Read-only by convention. Excluded from graph by user setting.
- Notion MCP authenticated as `miroslav.zachar@direct.cz`. Tools verified: `notion-get-users`, `notion-fetch`, `notion-search`, `notion-update-data-source`, `notion-query-data-sources`.
- Notion private Kanban DB: title "Second Brain", ID `35f18e00108d80c3985bcecb788758e2`, data source ID `35f18e00-108d-8068-86a6-000b6c14f3c0`. Active schema (post-shaping):
  - `Task name` (title)
  - `Status` (status type) — options: `Inbox | Today | Doing | Waiting | Done` (5, not 6 — Archive deferred; see "Open" below)
  - `Type` (select) — `delivery | code | learn | admin | people | decision`
  - `Due` (date) — renamed from `Due date`
  - `Assignee` (person) — vestigial from Notion Tasks template. Hidden in user's default view.
- Three skills written:
  - `.claude/skills/inbox/SKILL.md` — `/inbox` (renamed from `/triage` to avoid collision with Matt Pocock issue-tracker skill at parent `Projects\.claude\skills\triage`)
  - `.claude/skills/spar/SKILL.md` — `/spar`
  - `.claude/skills/draft/SKILL.md` — `/draft`
- Smoke test of `/inbox` ran: swept Notion Inbox, encountered 2 demo seed rows from Notion Tasks template, handled correctly (proposed discard, asked user confirmation, applied via `Status=Done` + `[discarded]` title prefix workaround).
- Auto-memory saved at `~\.claude\projects\c--Users-miroslav-zachar-OneDrive---Direct-Projects-scaffolding\memory\`:
  - `user_role.md`, `project_second_brain.md`, `feedback_no_rituals.md`, `MEMORY.md`.

### Open / pending user actions
1. **Notion UI**: add `Archive` option to Status property if user wants the 6-state board (current is 5-state, both work). Hide `Assignee` column in default view. Create 4 named views: `Board by Status`, `Today` (filter Status=Today), `Inbox` (filter Status=Inbox), `By Type` (board grouped by Type).
2. **First git push**: `git push -u origin main` from inside the dir. Hourly auto-push by Obsidian Git only starts after the initial push sets upstream.
3. **Old vault retirement**: remove `Dokumenty\Obsidian` from Obsidian's vault list (Obsidian → Manage Vaults → remove). The folder itself can stay on disk for safety; just stop opening it.

### Known limitations (durable — codify, don't keep re-discovering)
- **Notion typed-collection lock**: Tasks-template DBs in Notion mark certain properties as "required". The MCP cannot:
  - drop `Assignee` (typed property)
  - change `Status` from `status` type to anything else
  - mutate `status` property options via DDL (use Notion UI)
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
| Kanban schema | Minimal (Status / Type / Due) | Less = simpler. Deferred Priority, Hat, Project, Energy. Revisit after 2-3 weeks of real use. |
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
- View changes require Notion UI (MCP can't manage views).
- Status options on typed Status property require Notion UI (API limitation).

### Migrate to a fresh Notion DB (if typed-collection limits become painful)
The current DB inherits Notion's Tasks template, which locks `Assignee` and the Status property type. To escape:
1. Create new DB via `mcp__claude_ai_Notion__notion-create-database` under a regular (non-typed) parent page. Don't use Notion's pre-built Tasks template.
2. Migrate any active rows via `notion-create-pages` reading from old DB.
3. Update DB ID + data source ID in `CONTEXT.md` and `CLAUDE.md`.
4. Update `/inbox` skill if it refers to the old ID anywhere.

### Add a second Claude Code working dir (e.g., for a vibe-coding project)
Vibe-coding projects live as **sibling dirs** to `second-brain/` (e.g., `Projects\some-experiment\`), not inside it. Launch CC from inside the project dir. They will not see this dir's skills/CLAUDE.md unless you explicitly link.

## Files to be aware of

| File | Purpose | Edit when |
|--|--|--|
| `CLAUDE.md` | System instructions Claude reads on launch | You change persona, bindings, rules, skill list, or hard rules |
| `CONTEXT.md` | Live glossary including DB IDs and IDs of everything | Any new entity, role, term, or IDs change |
| `HOW-TO-USE.md` | User-facing operator manual | Daily workflow changes, new skills, new rules |
| `HANDOVER.md` (this file) | State snapshot for future CC sessions | After major changes, before context risk |
| `.claude/skills/<name>/SKILL.md` | Skill behavior | Behavior of that skill changes |
| `.gitignore` | Untracked paths | New machine-local files appear (settings.local.json, caches) |
| `vault/_templates/*.md` | Templater note templates | Note shape changes |

## Where the design conversation lives

The original `/grill-with-docs` session that produced this system is captured in:
- Plan file: `c:\Users\miroslav.zachar\.claude\plans\goal-of-this-session-glimmering-spark.md`
- Memory: `c:\Users\miroslav.zachar\.claude\projects\c--Users-miroslav-zachar-OneDrive---Direct-Projects-scaffolding\memory\`

If the user wants to re-grill on a part of the design (e.g., "let's reconsider the daily ritual"), the plan file is the audit trail of what was decided and rejected.

## Caveman mode note

The user runs with `caveman` mode often. SKILL.md files specify caveman as default for process talk. The draft output of `/draft` is normal prose (caveman doesn't bleed into deliverables). This is intentional — keep it that way.
