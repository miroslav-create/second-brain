# CONTEXT — domain glossary

> Evolving glossary for the second-brain workflow. Updated inline during `/spar` and other sessions when terms resolve. Each entry: term, one-line definition, optional note on boundaries.

## People

- **User / Miroslav** — Miroslav Zachar. Senior Business Analyst at **Fondee.cz** (Czech robo-advisor / ETF investment platform; part of **Direct** group). 27-person company, reports to CTO + CEO. Multi-hat: BA, PO/PM, Delivery Mgr, AI Ambassador, IT Analyst.

## Companies / orgs

- **Fondee.cz** — primary employer. Fintech (investment platform).
- **Direct** — corporate umbrella over Fondee + Pojišťovna Direct (Czech insurer). Email `@direct.cz` flows via this umbrella.
- **Pojišťovna Direct** — sister company under Direct umbrella. Hosts the shared Notion workspace.

## Tools

- **Workspace (Notion)** — `pojistovnadirect`, shared across Direct group. Public DBs visible to all ~27 staff; private spaces per user.
- **Team boards (Notion, public)** — Primary Kanban, DEV, Product, Meetings. Reference only.
- **Private Kanban (Notion)** — Miroslav's personal board in his private space. Schema `Status | Type | Due`. DB ID: _TBD after creation_.
- **Vault (Obsidian)** — markdown PKM at `Projects\second-brain\vault\`. Folders: daily, people, projects, evergreen, meetings, reference, canvas, _archive, _templates.
- **Old vault** — was at `Dokumenty\Obsidian`. Copied (read-only) to `vault\_archive\` on 2026-05-13. Excluded from graph view.
- **Claude Code** — invoked from `Projects\second-brain\` working dir.

## Workflow primitives

- **Task** — atomic unit on private Kanban. Min 15 min. Larger items broken into atoms in `vault/projects/` first; only atoms land on board.
- **Inbox** — two physical locations: Obsidian daily note `## Inbox` section, and Notion private Kanban `Inbox` column. Both drained by `/triage`.
- **Hat** — one of Miroslav's roles (BA, PO, PM, Delivery, AI-Amb, IT-Analyst). Not currently a Kanban property; surfaces in project notes.
- **Triage** — on-demand pass through both inboxes producing per-item decisions. Run via `/triage`. No fixed schedule.
- **Evergreen note** — atomic, self-contained concept note in `vault/evergreen/`. Written in user's words. Linked to other evergreens via `[[wikilinks]]`.
- **Daily note** — `vault/daily/YYYY-MM-DD.md`. Capture entry point + day's reasoning log + tomorrow nudge.
- **Project note** — `vault/projects/<slug>.md`. One per active piece of work. Holds why/outcome/stakeholders/open-questions/decisions/links.
- **Meeting note (Obsidian)** — `vault/meetings/meeting-YYYY-MM-DD-topic.md`. Long reasoning; bullets/actions go to Notion Meetings DB instead.

## Type values (Notion `Type` property)

- **delivery** — work output owed to others (spec, doc, decision).
- **code** — vibe-coding / side experiment / scripting.
- **learn** — read, try, take course.
- **admin** — timesheets, expenses, scheduling, IT chores.
- **people** — 1:1 prep, hiring, feedback, intentional conversations.
- **decision** — pending decision needing thought before status moves.

## Conventions

- **Overlap rule**: state machine → Notion. Prose + links → Obsidian.
- **Bridge**: when a Kanban task has an Obsidian breakdown, paste the note's URL/path in the task description body. No formal relation property.
- **Meeting split**: bullets → Notion Meetings DB; long reasoning → Obsidian linked via `[[meeting-YYYY-MM-DD-topic]]`.
- **No rituals**: triage and reviews are on-demand, never scheduled.
