---
name: notion-ticket
description: Create Notion tickets with CZ-first language conventions, initiative prefix enforcement, and layer prefix normalization. Explicit invocation only via /notion-ticket. Asks for missing context, drafts the ticket, shows confirm preview, writes after user approval. Does not auto-fire.
---

# /notion-ticket — guided Notion ticket creation

## Goal

Create a single Notion ticket that follows established conventions: CZ prose, EN-only tech terms, layer-prefixed title, initiative-derived ID prefix, sane property defaults. Confirm before write. No surprise writes.

## Invocation

Explicit only. Triggered by user typing `/notion-ticket <intent>` or `/notion-ticket` alone.

Examples:
- `/notion-ticket E5.8 batch retry on partial failure`
- `/notion-ticket new fadmin obrazovka pre lookup userov`
- `/notion-ticket` (no args — ask user for intent)

Do NOT auto-fire on phrases like "create ticket" / "add ticket". User must type the slash command.

## Out of scope

- Bulk creation (5+ tickets). Use direct Notion MCP calls instead.
- Editing existing tickets. Use direct Notion MCP `notion-update-page`.
- Deleting tickets.
- Voice / dictation.

## Flow

### 1. Parse intent

Extract from user-provided text:

- **Epic/sub-task hint** — pattern `E\d+(\.\d+)?` → e.g. `E5.8`, `E3`, `E7.4`
- **Layer hint** — keywords in text: `batch` / `BE` / `backend` → Backend; `app` / `mobil` / `mobile` → Mobil; `web` / `FE` / `frontend` → Web or Frontend; `content` / `obsah` / `FAQ` / `T&C` → Obsah; `fadmin` → Fadmin
- **Summary** — remaining text after stripping hints

If intent empty or ambiguous: ask user one focused question (intent description).

### 2. Resolve initiative

Determine which initiative the ticket belongs to:

- If `vault/initiatives/<slug>.md` currently open in IDE → use that slug
- Else scan recent conversation for `[[<slug>]]` mention or filename reference
- Else ask user: "Which initiative? (list available `vault/initiatives/*.md`)"

### 3. Derive ID prefix

For the resolved initiative slug:

1. Check initiative frontmatter for `ticket_prefix:` — if present, use that
2. Else derive: take first word of slug, strip dashes, uppercase first 3 letters
   - `referral-akce` → `REF`
   - `fondee-onboarding-v2` → `FON`
   - `kz-redesign` → `KZ` (or `KZR` if first word is 1-2 chars combine with next)
3. On first use per initiative, confirm derived prefix with user. After confirm, write to initiative frontmatter `ticket_prefix: <PREFIX>` so future calls skip the question.

### 4. Resolve epic + sub-task numbering

- If intent contains `E\d+\.\d+` (sub-task) → query Notion DB for existing `[<PREFIX>-E\d+\.\d+]` tickets to verify uniqueness. If number taken, ask user: bump to next or override.
- If intent contains `E\d+` only (epic) → similar uniqueness check
- If neither → ask user: "Epic-level or sub-task of which epic?"
- For sub-task: parent epic ticket URL (from Notion query) — store for parent linking after write.

### 5. Resolve layer

If layer hint detected → confirm with user (default to detected).

Layer set: **Backend / Frontend / Web / Mobil / Obsah / Batch / Fadmin**

If no hint → ask user (single-select).

### 6. Resolve DB target

Default: initiative-level. Initiative frontmatter may carry `notion_db:` (data source ID) for default target.

Known DBs:
- **`fromJIRA`** (team board) — data source `35e18e00-108d-816a-88e3-000baf8ced0a`. DB ID `35e18e00108d802fb960ed61bf982aba`. Used by `referral-akce`.
- **`Second Brain`** (personal triage) — data source `c0a6b990-bdb5-42e3-b112-6c8d424f3819`. DB ID `4f21d60dc0494d6f84e3248708e87af3`. Personal default per [[notion-netvory-mirror-board]] memory.

If no `notion_db:` in initiative frontmatter → ask user on first use, then persist to frontmatter.

### 7. Resolve properties

**Status:** default `Backlog`. Ask only if user signals otherwise.

**Tags:** infer from layer + always add `NEW`. Mapping:

| Layer | Notion Tag |
|--|--|
| Backend | `Back End` |
| Frontend | `FrontEnd` |
| Web | `FrontEnd` |
| Mobil | `Mobilní Appka` + `FrontEnd` |
| Obsah | (no specific tag) |
| Batch | `Back End` |
| Fadmin | `FADMIN` |

Plus heuristic: if intent mentions BUG → add `BUG` (and drop `NEW`); if mentions devops/CI/deploy → add `DevOPS`.

Confirm tag set with user before write.

**Priority / Estimation / Due:** skip unless user provides. Do not ask proactively.

**Project:** if initiative has matching Notion `Project` option mirroring slug → set. Else skip.

### 8. Draft ticket per language rules

Apply rules from [[feedback-notion-ticket-language]]:

- Title pattern: `[<PREFIX>-E<n>(.<m>)] <Layer>: <CZ description>`
- Description: CZ prose. EN reserved for: endpoint paths (`/validate-ref-code`), HTTP verbs (GET/POST), state names (IN_PROGRESS/ELIGIBLE/REWARDED), field/column names (display_name_revealed, state_history), framework names (FlatList/RecyclerView/UIActivityViewController), acronymy (KYC/GDPR/T&C/FAQ/PDF/FX/A11y/UI/UX/CSV/JSON), tech jargon without clean CZ equivalent (state machine, event handler, idempotency-key, cursor pagination, scaffold, hook, runbook, payout, batch, cron, audit log, migration, blocker).

Translate where CZ exists: validation → validace, logic → logika, list view → seznam, share → sdílení, empty state → prázdný stav, response → odpověď, decision → rozhodnutí, reveal → odhalení.

### 9. Show confirm preview

Format:

```
Draft ticket:
  DB: fromJIRA
  Title: [REF-E5.8] Batch: Retry pri parciálnom selhaní
  Description: <full CZ body>
  Status: Backlog
  Tags: Back End, NEW
  Project: referral-akce
  Parent: [REF-E5] Manuální batch proces výplaty odměn

Confirm? (y / e=edit / n=cancel)
```

User responses:
- `y` → proceed to write
- `e` → ask which field to edit, loop back to confirm
- `n` → cancel, no write

### 10. Write to Notion

Use `mcp__claude_ai_Notion__notion-create-pages` with resolved parent data source URL + properties + content.

After successful write:
- Return ticket URL
- Suggest: "Log to initiative `## Change log`? (y/n)" — if y, append one-liner via [[capture]] flow pattern.

## Hard rules

- Never write without confirm step (Q4 user requirement).
- Never auto-fire. Explicit `/notion-ticket` only (Q1 user requirement).
- Never duplicate existing ticket numbering. Query DB first.
- Never invent initiative slug. If unknown, ask.
- Never bypass language rules. CZ prose default, EN tech terms only.
- One ticket per invocation. Bulk = direct MCP.
- Date format: `YYYY-MM-DD` from environment, not invented.

## First-use bootstrapping

On first invocation per initiative, write to its frontmatter:

```yaml
ticket_prefix: REF
notion_db: 35e18e00-108d-816a-88e3-000baf8ced0a
```

This makes subsequent calls skip prefix + DB questions. User can edit frontmatter manually to override.

## Caveman default

Output terse per CLAUDE.md. Confirm preview = normal prose (deliverable). Process talk + question prompts = caveman.

## Cross-refs

- Language rules: [[feedback-notion-ticket-language]]
- Default DB memory: [[notion-netvory-mirror-board]]
- Capture flow (post-write log): [[capture]]
- Polished prose helper (if user wants longer description body): [[draft]]
