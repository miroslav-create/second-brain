# How to use your second-brain

> Operator's guide. Read this first. Bookmark it.
>
> **One-pager visual + cheat sheet**: [QUICKREF.md](QUICKREF.md). Use that to explain the system to someone in 30 seconds.

## The 30-second model

System optimizes for **living, interconnected project docs**. Pain it fixes: docs going stale, no change-impact view.

Three tools, three jobs:

| Tool         | Job                                                                    | When to open it                           |
|--------------|------------------------------------------------------------------------|-------------------------------------------|
| **Notion**   | Task source (Kanban: `Second Brain`). Tasks flow Inbox → Today → Done. | Capture tasks. Move through statuses.     |
| **Obsidian** | **Primary doc surface.** Initiative / domain / system notes. Graph = change-impact view. | Reading docs, viewing graph, manual edits |
| **Claude Code** | Partner + living-docs scribe. Listens during chat, batches captures at handover. | Sit down → work → say "prepare handover". |

**Overlap rule** (sharp): task state machine → Notion. Living prose + node graph → Obsidian. Daily ephemera → daily note (low signal, free-form).

---

## Three node types in vault

Everything in vault you'll be linking maps to one of three folders. Cap is three — don't add more node types without revisiting the design.

| Type           | Folder                  | What it is                                                    | Why it exists                                          |
|----------------|-------------------------|---------------------------------------------------------------|--------------------------------------------------------|
| **Initiative** | `vault/initiatives/`    | Named multi-week effort (e.g. `pis-migration`, `vendor-x-onboard`) | Holds why/scope/decisions/change log of one initiative |
| **Domain**     | `vault/domains/`        | Standing business area (e.g. `payments`, `data-platform`, `ops`)   | Cross-cutting node. Backlinks = which initiatives + systems live in this domain |
| **System**     | `vault/systems/`        | App, API, vendor, integration (e.g. `stripe`, `snowflake`, `vendor-z`) | Receives backlinks from every initiative touching it = "1 change → full scope" view |

Each note has frontmatter (`domain:`, `systems:`, `owner:`, `last_touched:` etc.). Backlinks do the heavy lifting in graph view.

---

## Daily flow

**No morning ritual. No thermometer. No fixed cadence.**

1. Sit down. Open Claude Code from this dir (`cd second-brain && claude`).
2. Work. Chat with CC about whatever you're doing. CC listens silently for change-verbs + node mentions.
3. When done for the day (or session): say *"prepare handover"* (or "wrap up" / "end of session" / explicit `/capture`).
4. CC presents batched review of captured items. You approve / edit / skip per item. Classify any new nodes (initiative / domain / system).
5. CC writes date-stamped one-liners to relevant notes. Quotes verbatim. Append-only.

Obsidian Git auto-pushes hourly. Vault stays in sync without thought.

---

## Capture flow — what triggers what

CC marks a candidate when **both** appear in one user utterance:

1. **Known node reference** — name matches a slug in `vault/initiatives|domains|systems/`. Plain text counts; no `[[ ]]` required.
2. **Change-verb** — one of: `updated, changed, decided, switched, owner-is, started, stopped, added, removed, deferred, replaced, deprecated, migrated, scoped, blocked, unblocked, fixed, broke, learned, found, agreed`.

CC also marks a **new-node candidate** when you mention a proper-noun-looking entity not yet in vault, alongside a change-verb. At handover, you classify it (i/d/s/skip).

**During session**: CC stays silent. No interrupts. No "should I log this?" mid-thought.

**At handover**: CC fires the batched review. Format:

```
N updates + M new nodes pending:

UPDATES
1. [[initiatives/pis-migration]] — "switched warehouse size to X-Small for cost"
2. [[systems/snowflake]] — "owner is now Eva"

NEW NODES (need classification: i=initiative / d=domain / s=system / skip)
3. vendor-z — "started integration scoping with Vendor Z"

approve all? edit individual (e:N)? skip individual (s:N)? classify new (3:s)?
```

Approve → CC appends to `## Change log` (and `## Decisions` on initiative notes when verb is `decided/agreed/scoped`). Bumps `last_touched` on system notes.

---

## On-demand use of Claude Code

Always launch from inside the workflow dir:

```
cd "c:\Users\miroslav.zachar\OneDrive - Direct\Projects\second-brain"
claude
```

Outside this dir, parent's `/triage` skill (Matt Pocock issue tracker) shadows yours.

### Skills you have

#### `/capture` — listen + batch + handover scribe (PRIMARY)
Active every turn, no invocation needed. Explicit `/capture` forces the batched review now. Full rules: `.claude/skills/capture/SKILL.md`.

#### `/inbox` — Notion Kanban Inbox sweep
Drains Notion Kanban rows where `Status == Inbox`. For each, proposes Type / Due / Status move. Daily-note inbox is no longer drained (you capture via chat now). When to run: when Notion Inbox column has rows you haven't dispositioned. Whenever you remember.

#### `/spar` — partner mode
Stress-tests a decision or design. One question at a time, with recommended answer. When to run: before big commits — vendor choice, OKR structure, system design, take/decline work.

#### `/draft` — bullets → prose
Polished draft for a destination — Notion page, Slack, email, spec section. When to run: notes ready, staring at blank page.

#### `/notion-ticket` — guided ticket creation
Creates one Notion ticket with CZ-first language + initiative prefix enforcement (e.g. `[REF-E5.8]`). Asks for missing context, drafts the title + description per language rules, shows confirm preview, writes after `y`. Explicit invocation only (no auto-fire). When to run: need a new ticket and want it to follow the conventions without manually typing the prefix + tags + status every time. Bulk creation (5+) = direct Notion MCP instead. Full rules: `.claude/skills/notion-ticket/SKILL.md`.

### Hand-offs
- `/capture` review surfaces a decision needing more thought → `/spar`.
- `/capture` captured item needs polished prose → `/draft`.
- `/spar` resolves → outcome may need `/draft`; follow-up task → `/inbox` or just say "started X" so capture picks it up.
- New ticket needed mid-session → `/notion-ticket <intent>`. After write, optionally log to initiative `## Change log` via capture flow.

---

## Working in Obsidian

- **Initiative note**: Templater → `initiative` template. Frontmatter: `domain`, `systems`, `stakeholders`, `hat`, `status`, `started`. Sections: Why / Scope / Open questions / Decisions / Change log / Links.
- **Domain note**: Templater → `domain` template. One per standing area. Frontmatter: `owner`. Sections: Mission / Active initiatives / Systems in scope / Decisions / Notes.
- **System note**: Templater → `system` template. Frontmatter: `owner`, `contract`, `last_touched`, `domain`. Sections: What / Contract / Owners / Gotchas / Consumers / Change log.
- **Daily note**: free-form ephemera. No structure imposed beyond minimal scaffold. Long-running prose belongs in initiative / domain notes.
- **Meeting note**: long reasoning → `vault/meetings/`. Bullet outcomes → Notion Meetings DB.
- **Evergreen note** (`vault/evergreen/`): atomic concept reusable across initiatives. Bottom-up only — never seed top-down.
- **`_archive/`**: read-only. Don't edit.

Link liberally with `[[ ]]`. Every initiative should link to its domain + systems. That's what makes graph view valuable.

---

## Working in Notion

DB: **Second Brain** (URL in `CONTEXT.md`). Task source only.

Columns:
- **Task name** — verb first
- **Status** — `Inbox` → `Today` → `Doing` → `Waiting` → `Done` → `Archive`
- **Type** — `delivery | code | learn | admin | people | decision`
- **Due** — only real deadlines
- **Project** — slug matches `vault/initiatives/<slug>.md` or `_unassigned_` for ad-hoc

Views: `Board by Status`, `Today`, `Inbox`, `By Type`, `By Project`.

**Closing a task**: when you set `Status=Done`, mention it in chat ("finished X for [[initiative-y]]") so capture flow logs it to the initiative note. No formal forced-doc step yet — relies on conversational mention.

---

## Graph view — your mind map

Open Obsidian → graph icon in left ribbon.

Renders network of `[[wikilinks]]`. To make useful:
- Every initiative frontmatter lists domain + systems → instant edges.
- System notes accumulate backlinks from initiatives → change-impact view ("touch Stripe → 4 initiatives ride on it").
- Domain notes accumulate edges from initiatives + systems → business-area overview.

Graph is emergent. Don't plan it. Glance monthly. Spot orphan nodes, cluster gaps.

---

## What NOT to do

- Don't write thinking / prose inside Notion. Notion = state. Obsidian = thinking.
- Don't duplicate team-board tickets (Primary, DEV, Product) into private Kanban. Link only.
- Don't put recurring routines on board. Use calendar.
- Don't use skills on a schedule. Invoke when you want help.
- Don't edit `vault/_archive/`. Copy clean version into `vault/evergreen/` if you find gold.
- Don't auto-write to vault during session. Capture batches at handover.
- Don't add a 4th node type without revisiting design. Three is the cap.

---

## When something breaks

- **Notion MCP unauthorized** → run `mcp__claude_ai_Notion__authenticate`, complete browser flow.
- **Notion API rate-limited (429)** → wait 30s, re-run. CC retries once automatically.
- **Obsidian Git not committing** → check plugin status, manual `git status` from dir.
- **Wrong `/triage` loaded** → launched CC outside `second-brain/`. Quit, `cd` in, relaunch.
- **`/capture` review empty when you expect items** → check change-verb list in `.claude/skills/capture/SKILL.md`. Tune list or use explicit "log to [[node]]: <text>" phrasing as override.

---

## Where things live

```
c:\Users\miroslav.zachar\OneDrive - Direct\Projects\second-brain\
├── HOW-TO-USE.md         ← this file
├── HANDOVER.md           ← state of system, for future CC sessions
├── CLAUDE.md             ← Claude reads this on launch
├── CONTEXT.md            ← live glossary (people, terms, IDs)
├── docs/                 ← frozen historical records
├── vault/                ← Obsidian vault
│   ├── initiatives/      ← named multi-week efforts (PRIMARY)
│   ├── domains/          ← standing business areas (PRIMARY)
│   ├── systems/          ← apps / APIs / vendors (PRIMARY)
│   ├── daily/            ← free-form ephemera
│   ├── people/           ← one note per colleague
│   ├── projects/         ← legacy, kept for backward compat (prefer initiatives/)
│   ├── evergreen/        ← atomic concept notes (bottom-up only)
│   ├── meetings/         ← long-form meeting reasoning
│   ├── reference/        ← personal docs / how-tos
│   ├── canvas/           ← .canvas spatial diagrams
│   ├── _archive/         ← old vault, read-only
│   └── _templates/       ← Templater templates (incl. initiative/domain/system)
├── .claude/
│   ├── settings.json     ← project-level perms baseline
│   ├── settings.local.json  ← machine-local perms (gitignored)
│   └── skills/
│       ├── capture/        ← /capture skill (PRIMARY, auto-active)
│       ├── inbox/          ← /inbox skill (Notion Inbox drain)
│       ├── spar/           ← /spar skill
│       ├── draft/          ← /draft skill
│       └── notion-ticket/  ← /notion-ticket skill (guided Notion ticket creation)
└── inbox-cache/          ← scratch space (gitignored)
```

---

## Tuning over time

After 2-3 weeks of real use:
- **Change-verb list too noisy?** Trim in `.claude/skills/capture/SKILL.md`.
- **Change-verb list missing your idioms?** Add verbs you actually use.
- **Three node types not enough?** Probably no — push back hard against adding more. Cross-cutting concerns usually fit into domain notes.
- **Graph view too dense / too sparse?** Check link discipline. Initiatives should always link domain + systems. Add missing.
- **Daily-note completely empty?** Fine. Drop it from your habit. Daily notes are optional now.
- **Notion Inbox piling up?** That's the only canary signal worth keeping. Run `/inbox`.

To make tweaks: see `HANDOVER.md`.
