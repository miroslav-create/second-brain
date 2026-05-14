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
- **Team boards (Notion, public)** — reference only, never duplicate into private Kanban.
  - Primary Kanban: `28818e00108d809dbc90d542a6ace557`
  - DEV: `2b518e00108d80bc9d44e8ebe005118a`
  - Product: `27218e00108d807ca79bd7023d89c8ef`
  - Meetings: `28b18e00108d80c0a6bbc208b2801f3d`
- **Notion MCP access** — `mcp__claude_ai_Notion__*` tools.
- **Private Kanban (Notion)** — Miroslav's personal board "Second Brain" in his private space.
  - Database ID: `4f21d60dc0494d6f84e3248708e87af3`
  - Data source ID: `c0a6b990-bdb5-42e3-b112-6c8d424f3819`
  - URL: https://www.notion.so/4f21d60dc0494d6f84e3248708e87af3
  - Schema: `Task name` (title), `Status` (select), `Type` (select: delivery|code|learn|admin|people|decision), `Due` (date), `Project` (select: mirrors `vault/projects/<slug>.md` filenames; starts with `_unassigned_` placeholder).
  - Status options: `Inbox | Today | Doing | Waiting | Done | Archive`. Fully API-mutable via DDL (plain DB, not Tasks template). Status filter on views binds correctly via MCP DSL.
  - Migrated 2026-05-13 from old typed-Tasks-template DB (escaped typed-collection lock). Old DB ID: `35f18e00108d80c3985bcecb788758e2` (archived).
  - **MCP limitation (durable)**: Notion MCP does not expose page-archive or page-trash actions. To remove rows from sight without UI: set `Status=Done` (or `Archive`) and prefix title with `[discarded]`. True trash = user action in Notion UI.
- **Vault (Obsidian)** — markdown PKM at `Projects\second-brain\vault\`. Folders: **initiatives, domains, systems** (primary node types), daily, people, projects (legacy), evergreen, meetings, reference, canvas, _archive, _templates.
- **Old vault** — was at `Dokumenty\Obsidian`. Copied (read-only) to `vault\_archive\` on 2026-05-13. Excluded from graph view.
- **Claude Code** — invoked from `Projects\second-brain\` working dir.

## Workflow primitives

### Vault node types (PRIMARY — three cap)

- **Initiative note** — `vault/initiatives/<slug>.md`. Named multi-week effort. Frontmatter: `domain`, `systems`, `stakeholders`, `hat`, `status`, `started`. Sections: Why / Scope / Open questions / Decisions / Change log / Links. Capture flow appends date-stamped entries to Decisions + Change log.
- **Domain note** — `vault/domains/<slug>.md`. Standing business area (payments, data-platform, ops, people, etc.). Cross-cutting node. Frontmatter: `owner`. Sections: Mission / Active initiatives / Systems in scope / Decisions / Notes. Backlinks panel reveals initiatives + systems living in this domain.
- **System note** — `vault/systems/<slug>.md`. App, API, vendor, integration. Frontmatter: `owner`, `contract`, `last_touched`, `domain`. Sections: What / Contract / Owners / Gotchas / Consumers / Change log. Backlinks from initiatives = "1 change → full scope" view.

### Other vault entities

- **Daily note** — `vault/daily/YYYY-MM-DD.md`. Free-form ephemera. No imposed structure. Thermometer + structured inbox removed 2026-05-14.
- **Evergreen note** — `vault/evergreen/<slug>.md`. Atomic concept reusable across initiatives. Bottom-up emergent only — never seed top-down.
- **Meeting note** — `vault/meetings/meeting-YYYY-MM-DD-topic.md`. Long reasoning; bullets/actions go to Notion Meetings DB.
- **Project note** (legacy) — `vault/projects/<slug>.md`. Pre-redesign convention. New work goes to `vault/initiatives/<slug>.md` instead. Kept for backward compat; migrate when convenient.

### Task / capture primitives

- **Task** — atomic unit on private Kanban. Min 15 min. Larger items broken into initiative-note prose first; only atoms land on Kanban.
- **Notion Inbox** — `Status=Inbox` column on private Kanban. Drained on-demand by `/inbox`.
- **Capture candidate** — in-session record produced by `/capture` skill when `[change-verb] + [node-reference]` detected in user utterance. Stored with verbatim trigger quote. Confirmed at handover.
- **Change-verb** — see `.claude/skills/capture/SKILL.md` for current list. Trigger for capture detection.
- **Handover** — natural-language session-end signal ("prepare handover" / "wrap up" / "end of session" / explicit `/capture`). Fires batched capture review.
- **New-node candidate** — proper-noun-looking entity mentioned alongside change-verb, no slug match in vault. Classified at handover (i/d/s/skip).
- **Hat** — one of user's roles (BA, PO, PM, Delivery, AI-Amb, IT-Analyst). Surfaces in initiative-note frontmatter (`hat:`). Not a Kanban property.

## Type values (Notion `Type` property)

- **delivery** — work output owed to others (spec, doc, decision).
- **code** — vibe-coding / side experiment / scripting.
- **learn** — read, try, take course.
- **admin** — timesheets, expenses, scheduling, IT chores.
- **people** — 1:1 prep, hiring, feedback, intentional conversations.
- **decision** — pending decision needing thought before status moves.

## Conventions

- **Overlap rule**: task state machine → Notion (task source). Living prose + node graph → Obsidian (primary doc surface). Daily ephemera → daily note.
- **Bridge**: when a Kanban task has an Obsidian initiative-note, paste the note's URL/path in the task description body. No formal relation property.
- **Meeting split**: bullets → Notion Meetings DB; long reasoning → Obsidian linked via `[[meeting-YYYY-MM-DD-topic]]`.
- **No rituals**: capture, sweeps, sparring all on-demand. Capture fires on user handover-intent only.
- **Three-node cap**: vault node types capped at initiative/domain/system. Push back hard before adding a fourth.
- **Notion `Project` slug**: currently mirrors `vault/projects/<slug>.md` (legacy). Migrate to `vault/initiatives/<slug>.md` convention at first real initiative birth.
