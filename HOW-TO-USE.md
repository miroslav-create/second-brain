# How to use your second-brain

> Operator's guide. Read this first. Bookmark it.

## The 30-second model

Three tools, three jobs:

| Tool         | Job                                                  | When to open it                           |
|--------------|------------------------------------------------------|-------------------------------------------|
| **Notion**   | Tasks with a status (Kanban). One DB: `Second Brain`. | Capture work-tasks. Move them through states. |
| **Obsidian** | Thinking, writing, accumulating knowledge.           | Sit-down notes. Project breakdowns. Mind map (graph view). |
| **Claude Code** | Partner. Drains your inboxes. Sparring. Drafting. | When you want help — not on a schedule.   |

**The overlap rule** (sharp): state machine → Notion. Prose + links → Obsidian. If you're unsure which, ask: "does this need to move through statuses?" Yes → Notion. No → Obsidian.

---

## Daily capture — where things go in the moment

You'll be in one of four situations when an idea/task hits. Each has a target:

| Situation                                | Capture here                                                          |
|------------------------------------------|------------------------------------------------------------------------|
| Sitting at desk, working                 | Notion mobile/web → `Second Brain` DB → new row → leave Status=Inbox  |
| Sitting somewhere thinking, journaling   | Obsidian → today's daily note → drop bullet under `## Inbox`           |
| On phone, walking, in the shower         | Type into Notion mobile (work) or Obsidian mobile (thinking)           |
| In a meeting                             | Bullet outcomes → Notion **Meetings DB** (team). Long reasoning → Obsidian `vault/meetings/meeting-YYYY-MM-DD-topic.md` |

**No voice.** No transcription pipeline. Type or skip.

---

## Daily / on-demand use of Claude Code

There is **no fixed ritual**. Open Claude Code when you want help. Always launch it from inside the workflow dir:

```
cd "c:\Users\miroslav.zachar\OneDrive - Direct\Projects\second-brain"
claude
```

That second step matters — from `scaffolding/` or other dirs, the parent's `/triage` skill (Matt Pocock issue tracker) shadows your skills.

### Three skills you have

#### `/inbox` — clear your inboxes
What it does: reads today's Obsidian daily note `## Inbox` + Notion `Second Brain` rows where Status=Inbox. For each item proposes:
- Become a Notion task (sets Type, optionally Due, leaves Status=Inbox or moves to Today)
- Become an Obsidian evergreen note (extracted from inbox into `vault/evergreen/<slug>.md`)
- Discard (Notion: Status=Done + `[discarded]` prefix; Obsidian: strike-through)

It shows you a table, you confirm "all", "skip 3", "change 5 to discard", etc.

**When to run it:** when your inboxes feel cluttered. Could be once a day, twice a week, every Sunday — whenever.

**Pre-req:** today's Obsidian daily note must exist. Open Obsidian → command palette (Ctrl+P) → "Daily notes: Open today's daily note". Templater creates the file with the right shape.

#### `/spar` — partner mode
What it does: stress-tests a decision or design. Asks questions one at a time, each with a recommended answer. You push back, change direction, agree. Updates `CONTEXT.md` glossary as terms resolve.

**When to run it:** before big commits — picking a vendor, structuring an OKR, sketching a system design, deciding to take on / decline work. Anything where "talk it out" helps.

**Tip:** start with one paragraph stating what you're thinking about. Don't write a polished proposal — give raw thoughts and let the skill drag clarity out.

#### `/draft` — bullets → prose
What it does: takes your messy bullets/notes and produces a polished draft for a specific destination — Notion page, Slack message, internal email, external email, spec section, Kanban task description.

**When to run it:** when you have notes but staring at the page. "Make me a 3-paragraph email to Anna telling her X" → done in 20 sec. Always ask CC about audience, tone, language (CZ/EN) once if unclear.

---

## Working in Obsidian — a few rules

- **Daily note**: create via command palette ("Daily notes: Open today's daily note"). Never paste in your own daily file shape — Templater frontmatter matters.
- **Evergreen note**: create via Templater → pick `evergreen` template. Title = the concept. One atomic idea per note. Link to other evergreens with `[[wikilinks]]` — that's what makes the graph view valuable.
- **Project note**: one per active project in `vault/projects/`. Use Templater → `project` template. Holds why/outcome/stakeholders/open-questions/decisions.
- **Meeting note**: long reasoning only. Bullet outcomes belong in Notion Meetings DB.
- **`_archive/`**: read-only. Old Fondee vault from before fresh start. Search it, don't edit it. Excluded from graph view via Settings → Files & Links → Excluded files → `_archive`.

---

## Working in Notion — what's on the board

DB: **Second Brain** (URL in `CONTEXT.md`). One row per task.

Columns to set:
- **Task name** — what you'll do, verb first if possible
- **Status** — `Inbox` (just landed) → `Today` (committed to today) → `Doing` (currently doing) → `Waiting` (blocked, note who/what in description) → `Done`
- **Type** — `delivery | code | learn | admin | people | decision`
- **Due** — only set if it's a real deadline (don't invent due dates)

Columns to ignore: **Assignee** (left in by Notion template, hidden in views — single-user board).

If a task has a long breakdown, paste a link/URL to the Obsidian note in the task description body. No formal relation property.

---

## The graph view — your mind map

Open Obsidian → click the graph icon in the left ribbon.

It auto-renders the network of `[[wikilinks]]` between notes. To make it useful:
- Link liberally. Every time you mention a person, project, or concept that has its own note, wrap it in `[[ ]]`.
- Hub notes are OK — make `[[Fondee]]`, `[[OKR Q3 2026]]` etc. that act as anchors many leaves link to.
- The graph is **emergent**, not designed. Don't try to plan it — just link as you write, look at it monthly to spot clusters and gaps.

---

## Mental model — what NOT to do

- Don't duplicate work-tracked tickets from team boards (Primary, DEV, Product) into your private Kanban. Reference them with a link. They're already tracked.
- Don't put recurring routines (weekly review, monthly reports) on the board. Use a calendar or habit tracker. The Kanban is for things you *do*, not things you *repeat*.
- Don't write notes inside Notion when the content is "thinking, prose, links". Notion is for state. Obsidian is for thinking.
- Don't use a skill on a schedule. Invoke when you want help, not because the clock ticked.
- Don't edit `vault/_archive/`. If you find gold in there, copy the cleaned version into `vault/evergreen/` with proper frontmatter — leave the original for provenance.

---

## When something breaks

- **`/inbox` says daily note missing** → open Obsidian, create today's daily note via Templater, re-run.
- **Notion MCP unauthorized** → run `mcp__claude_ai_Notion__authenticate` and complete browser flow.
- **Notion API rate-limited (429)** → wait 30 seconds, re-run; CC will retry automatically once.
- **Obsidian Git not committing** → check plugin status in Obsidian settings → Community plugins. Manual `git status` from the dir to see what's pending.
- **Wrong `/triage` loaded** (Matt Pocock issue-tracker fires instead of your skill) → you launched CC from outside `second-brain/`. Quit, `cd` in, relaunch. Inside the dir use `/inbox` (renamed).

---

## Where things live

```
c:\Users\miroslav.zachar\OneDrive - Direct\Projects\second-brain\
├── HOW-TO-USE.md         ← this file
├── HANDOVER.md           ← state of system, for future CC sessions
├── CLAUDE.md             ← Claude reads this on launch
├── CONTEXT.md            ← live glossary (people, terms, IDs)
├── vault/                ← your Obsidian vault
│   ├── daily/            ← daily notes
│   ├── people/           ← one note per colleague
│   ├── projects/         ← project breakdowns
│   ├── evergreen/        ← atomic concept notes (the PKM)
│   ├── meetings/         ← long-form meeting reasoning
│   ├── reference/        ← personal docs / how-tos
│   ├── canvas/           ← .canvas spatial diagrams
│   ├── _archive/         ← old vault, read-only
│   └── _templates/       ← Templater templates
├── .claude/
│   └── skills/
│       ├── inbox/        ← /inbox skill
│       ├── spar/         ← /spar skill
│       └── draft/        ← /draft skill
├── scripts/              ← glue scripts (mostly empty for now)
└── inbox-cache/          ← scratch space (gitignored)
```

---

## Tuning over time

After 2–3 weeks of real use, you'll feel which corners are wrong. Things to revisit:
- Are inboxes accumulating because you forget to run `/inbox`? Consider a Friday afternoon habit (still no automation — just human discipline).
- Is the minimal schema (Status, Type, Due) too thin? Add Priority or Hat properties.
- Is anything still landing in old vault paths? Verify Obsidian shows only the new vault.
- Is `/draft` missing a destination? Add more options to its SKILL.md.

To make those tweaks: see `HANDOVER.md`.
