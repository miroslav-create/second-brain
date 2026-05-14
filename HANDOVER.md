# Handover — second-brain workflow

> For future Claude Code sessions picking up this system. Read this if user says "let's tweak the workflow" or "something's off with X".

## TL;DR

Personal workflow integrating **Notion (task source)** + **Obsidian (living docs + change-impact graph)** + **Claude Code (partner + silent capture scribe)**. Redesigned 2026-05-14 around user's actual pain: **stale/missing project documentation** and lack of "one change → full scope" visibility.

Earlier scaffold (built 2026-05-13) over-rotated on task-hygiene (inbox thermometer, dual-inbox drain). User grilled in this session — pain isn't task hygiene; pain is docs going stale. Pivot:

- **Three vault node types**: `initiatives/`, `domains/`, `systems/`. Graph view shows change-impact via backlinks.
- **Capture mechanics**: CC listens silently every turn for `[change-verb] + [node-reference]` pattern. Batches candidates with verbatim trigger quotes. Fires review when user says "prepare handover" / similar / explicit `/capture`. User confirms appends + classifies any new nodes.
- **Retired**: thermometer line, dual-inbox drain, morning ritual debate, primary-Kanban framing.
- **Kept**: Notion DB as task source, `/spar`, `/draft`, Obsidian Git auto-push, daily notes (demoted to free-form ephemera).

The system is **live and seedless** — user opted to populate initiatives/domains/systems incrementally as they get mentioned in chat, rather than dumping a seed list up front.

## How to operate

For the one-page visual + cheat sheet: [`QUICKREF.md`](QUICKREF.md).
For the user-facing daily/weekly use: [`HOW-TO-USE.md`](HOW-TO-USE.md).
For the user/CC operating contract: [`CLAUDE.md`](CLAUDE.md).
For the live glossary: [`CONTEXT.md`](CONTEXT.md).
For capture rules (verb list, trigger phrases, format): [`.claude/skills/capture/SKILL.md`](.claude/skills/capture/SKILL.md).

## Current state (live as of 2026-05-14)

### What's live

- **Filesystem scaffold** under `c:\Users\miroslav.zachar\OneDrive - Direct\Projects\second-brain\` (OneDrive-synced). Git on `main`, tracks `origin/main`. Hourly Obsidian Git auto-push operational.
- **Obsidian vault** at `vault/`. Templater + Obsidian Git installed. Daily Notes core plugin → template `_templates/daily.md`. Old Fondee vault archived read-only at `vault/_archive/`, excluded from graph.
- **Three node-type folders** seeded empty:
  - `vault/initiatives/` — named multi-week efforts
  - `vault/domains/` — standing business areas
  - `vault/systems/` — apps / APIs / vendors / integrations
- **Three templates** in `vault/_templates/`: `initiative.md`, `domain.md`, `system.md`. Each prompts for slug, renames + moves file, seeds frontmatter + section scaffold.
- **Capture skill** at `.claude/skills/capture/SKILL.md`. Auto-active every user turn. Silent during session. Fires batched review on handover intent or explicit `/capture`. Detection: `[change-verb] + [node-reference]`. New-node candidates flagged for classification.
- **Notion MCP** authenticated as `miroslav.zachar@direct.cz`.
- **Notion private Kanban DB**: title `Second Brain`, ID `4f21d60dc0494d6f84e3248708e87af3`, data source `c0a6b990-bdb5-42e3-b112-6c8d424f3819`. Plain DB schema (no Tasks template): `Task name`, `Status` (select, options `Inbox|Today|Doing|Waiting|Done|Archive`), `Type` (select), `Due` (date), `Project` (select, options mirror `vault/initiatives/<slug>.md` filenames).
- **Skills** (`.claude/skills/`): `capture` (PRIMARY, auto-active), `inbox` (Notion-Inbox-only sweep — shrunk 2026-05-14 from earlier dual-inbox drain), `spar`, `draft`.
- **Daily template** (`vault/_templates/daily.md`) stripped of thermometer line. Now free-form ephemera.
- **Settings**: project-level `.claude/settings.json` committed (Notion read+write tools, read-only git). Destructive perms in `.claude/settings.local.json` (gitignored).
- **Auto-memory** at `~\.claude\projects\c--Users-miroslav-zachar-OneDrive---Direct-Projects-second-brain\memory\`.

### Open / pending user actions

1. **Seed initiatives / domains / systems incrementally** — no seed dump. As user works + mentions things in chat, new-node candidates surface at handover. User classifies (i/d/s). Stub created from template.
2. **Notion UI — rename old DB to `[archived] Second Brain v1`**. URL: https://www.notion.so/35f18e00108d80c3985bcecb788758e2 — backup window 30 days then delete. (API can't rename typed-Tasks-template DB title.)
3. **Notion UI — rename `_unassigned_` Project option** to first real initiative slug when one lands. Or DDL drop once at least one real option exists.
4. **Notion Project options vs initiative folder** — `Project` property currently mirrors `vault/projects/<slug>.md` filenames (legacy). New initiatives go into `vault/initiatives/<slug>.md`. Decide: keep `Project` pointing at `vault/projects/` (legacy compat) or migrate to `vault/initiatives/` slug convention. Recommend migrate at first real initiative birth — update one slug, update one Notion option, done.

### Live views on private Kanban

| Name | Type | View ID |
|--|--|--|
| Board by Status | board, GROUP BY Status | `35f18e00-108d-81fc-8cfa-000cb2d28097` |
| Today | board, GROUP BY Status, filter Status=Today | `35f18e00-108d-81bc-9f1e-000c3910dce3` |
| Inbox | board, GROUP BY Status, filter Status=Inbox | `35f18e00-108d-81ec-8657-000c3181a89d` |
| By Type | board, GROUP BY Type | `35f18e00-108d-817b-9c4a-000c0ed05add` |
| By Project | board, GROUP BY Project | `35f18e00-108d-8142-a73e-000cdcef4d21` |

### Old DB (archived) — `35f18e00108d80c3985bcecb788758e2`
Pre-migration. Rename + delete after 30-day safety window.

### Known limitations (durable — codify, don't keep re-discovering)

- **Notion typed-collection lock**: Tasks-template DBs mark properties "required". Old DB hit this; new DB is plain → escaped. Don't recreate via Tasks template.
- **Notion MCP DSL — status filter gap**: on `status`-type property, `FILTER "<status-property>" = "<option>"` silently produces empty `advancedFilter.filters`. On `select`-type, binds correctly. Current DB uses `select`, no issue.
- **Notion MCP DSL — `simpleFilters` legacy schema not writable**: views created in Notion UI may carry filters under legacy `simpleFilters`. MCP writes only to `advancedFilter`. Can't fix/remove legacy filters via API. UI required.
- **No view delete via MCP**: `notion-update-view` has no trash flag. Duplicate views deleted via UI. Mitigation: rename via MCP to `[dup-delete]` prefix.
- **No view-type setter via MCP**: can't flip board↔table via API. UI Layout selector only.
- **Notion MCP archive/trash gap**: no page-archive/trash exposed. To "discard": set `Status=Done`, prefix title `[discarded] `. True trash = UI action.
- **Skill name collision**: parent `Projects\.claude\skills\triage` is Matt Pocock's issue-tracker skill. Shadows `/triage` when CC launched outside `second-brain/`. Our drain skill is named `/inbox`. Always launch from inside this dir.
- **No voice / transcription**: explicitly rejected by user.
- **No automation / scheduling**: user explicitly rejected fixed-time rituals. Do not propose cron, ScheduleWakeup, recurring routines. See memory file `feedback_no_rituals.md`.
- **Templater is GUI-bound**: CC writing template-rendered notes must substitute slug + date manually (write plain markdown matching template body). User-driven note creation via Templater command palette only.

## Design decisions (2026-05-14 redesign)

Resolved during grill session this date. If a change feels like it touches one of these, it's reopening a decision — be explicit with user.

| Decision | Choice | Why |
|--|--|--|
| Primary pain | Stale/missing project docs + no change-impact view | User's words. Not task hygiene. Not context-switch tax. Not capture-friction. |
| Atom of documentation | Project note ("section appended to existing project note") | User picked A on Q3. Other atoms (evergreen / system / meeting) are secondary/derivative. |
| Update trigger | Real-time chat catch (silent) + Notion-close mention | Picked B+D. (a) end-of-day nudge = ceremony reborn. (c) on-demand = same staleness as today. |
| Project granularity | Initiatives + standing domain buckets ("mixed") | Picked D. Pure initiative-scale leaves emergent work homeless. Pure domain-bucket loses initiative-level decision history. |
| Systems as node type | Own notes (not just frontmatter tags) | Picked A. Pain is *stale docs of systems*. Dedicated node = home for API quirks, owners, contracts. Backlinks give "1 change → full scope". |
| Capture interruption model | Batched at session end | Picked C, against recommendation. Mitigation: CC stores trigger-quote with each candidate to preserve context across long sessions. |
| Session-end trigger | Natural-language intent ("prepare handover" / similar) | User's words. No `/log` command required (though `/capture` works as explicit override). |
| New-node birth | One-confirm classification at handover | Picked B. Avoid auto-stub junk from misspeak. Keypress cost trivial; misclassified node-type sticky. |
| Change-verb list | Default: updated/changed/decided/switched/owner-is/started/stopped/added/removed/deferred/replaced/deprecated/migrated/scoped/blocked/unblocked/fixed/broke/learned/found/agreed | Silent-accept. Trimmable later. |
| Daily notes | Keep, demote to free-form ephemera | Useful for "what did I do today" snapshot. Thermometer + structured inbox section dropped. |
| Thermometer | Dropped | New goal isn't triage discipline. Measurement irrelevant to docs-staleness pain. |
| `/inbox` skill | Shrunk to Notion-Inbox-only | Daily-note inbox drain redundant — capture flow handles emergent work. Notion Inbox column still needs occasional drain. |
| Notion role | Demoted to "task source" | Was framed as primary surface in earlier design. Obsidian is primary now. |
| Caveman default | Kept on for process talk. Code/commits/security: normal English. | User runs caveman often. SKILL.md outputs caveman. Deliverables (drafts) stay normal prose. |

## What was retired (2026-05-14)

- **Thermometer line** in `vault/_templates/daily.md` — removed. New goal != triage discipline.
- **Open question "morning flow too heavy"** — moot. No morning ritual exists. User goal: sit→work→handover.
- **Dual-inbox drain in `/inbox`** — daily-note inbox no longer drained. `/inbox` shrunk to Notion Kanban Inbox column only.
- **"Notion Kanban as primary surface" framing** in `HOW-TO-USE.md` — replaced with "task source" role.
- **Earlier `Project` slug convention pointing at `vault/projects/`** — pending decision (see Open #4). Likely migrate to `vault/initiatives/` at first real initiative birth.

## How to extend or modify

### Add a new skill
Create `.claude/skills/<name>/SKILL.md` with frontmatter `name:` + `description:`. Description must explicitly say when to invoke. Body = logic. Auto-picked up by CC launched from this dir.

### Add a new node type to vault
**Push back hard first.** Cap is three (initiative / domain / system) by design. Cross-cutting concerns usually fit into domain notes. If user insists:
1. Create `vault/<type>s/` folder.
2. Write `vault/_templates/<type>.md` template (mirror existing template shape).
3. Update `.claude/skills/capture/SKILL.md` detection rule to scan new folder.
4. Update `CLAUDE.md` + `HOW-TO-USE.md` + this file.

### Add Notion Kanban property
Use `mcp__claude_ai_Notion__notion-update-data-source` with `ADD COLUMN`. Example:
```
ADD COLUMN "Priority" SELECT('P0':red, 'P1':orange, 'P2':yellow, 'P3':gray)
```
Update `CONTEXT.md` schema section. Update `/inbox` SKILL.md if new property set during sweep.

### Change capture rules (verbs, trigger phrases, format)
Edit `.claude/skills/capture/SKILL.md`. Rules are explicit there — no hidden state.

### Tune change-verb list
Same file. Trim if noisy. Add if user's idioms missing. Survey actual capture-review output after a few sessions to decide.

### Migrate to another fresh Notion DB (if ever needed)
1. Create new DB via `mcp__claude_ai_Notion__notion-create-database` — schema DDL, no parent for workspace-level.
2. Migrate active rows via `notion-create-pages` (read old via `notion-query-data-sources` SQL).
3. Update DB ID + data source ID in `CONTEXT.md`.
4. Recreate views via `notion-create-view`. Verify filters bind by fetching new DB.
5. Rename old DB `[archived] <name>` in UI; delete after 30-day window.

### Add a second CC working dir
Sibling dirs to `second-brain/`, not nested. Launch CC from inside the project dir. They won't see this dir's skills/CLAUDE.md unless explicitly linked.

## Files to be aware of

| File | Purpose | Edit when |
|--|--|--|
| `CLAUDE.md` | System instructions CC reads on launch | Persona, capture rules, hard rules change |
| `CONTEXT.md` | Live glossary, DB IDs, node-type schema | New entity, term, IDs change |
| `HOW-TO-USE.md` | User-facing operator manual | Daily flow, new skills, new rules |
| `HANDOVER.md` (this) | State snapshot for future CC sessions | After major changes, before context risk |
| `docs/design-*.md` | Frozen historical design plans | Never — audit-trail records |
| `.claude/settings.json` | Project-level perms baseline (committed) | New tool needs perm at project level |
| `.claude/settings.local.json` | Machine-local perms (gitignored) | New tool needs perm for this machine only |
| `.claude/skills/<name>/SKILL.md` | Skill behavior | Behavior of that skill changes |
| `.claude/skills/capture/SKILL.md` | **Capture rules — most-edited file when tuning capture flow** | Verb list, trigger phrases, format, detection rule changes |
| `vault/_templates/*.md` | Templater note templates | Note shape changes |

## Where design conversations live

- **2026-05-13 original `/grill-with-docs`** (in-repo frozen): [`docs/design-2026-05-13.md`](docs/design-2026-05-13.md)
- **2026-05-13 review + migration session**: `c:\Users\miroslav.zachar\.claude\plans\let-s-review-everything-start-declarative-nest.md`
- **2026-05-14 redesign session** (this — pain reframed to docs-staleness, capture mechanics): conversational only; this `HANDOVER.md` is the audit trail. If a future re-grill challenges a 2026-05-14 decision (e.g. "let's reconsider batched-vs-realtime"), the Design decisions table above is the trail.

Memory (auto-saved):
- `c:\Users\miroslav.zachar\.claude\projects\c--Users-miroslav-zachar-OneDrive---Direct-Projects-second-brain\memory\`
- `c:\Users\miroslav.zachar\.claude\projects\c--Users-miroslav-zachar-OneDrive---Direct-Projects-scaffolding\memory\` (legacy path from earlier rename — may exist)

## Caveman mode note

User runs `caveman` mode often. SKILL.md files specify caveman default for process talk. `/draft` output stays normal prose (caveman doesn't bleed into deliverables). Capture-flow review output: caveman list + confirm prompt. Intentional. Keep.
