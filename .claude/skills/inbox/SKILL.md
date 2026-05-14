---
name: inbox
description: Drain Notion private Kanban Inbox column. Propose disposition per row (set Type / Due / Status, or discard). User confirms batch. Daily-note inbox section is no longer drained — emergent work flows through /capture instead. Invoke when user types /inbox.
---

# /inbox — Notion Kanban Inbox sweep

## Scope (post-2026-05-14 redesign)

Drains **Notion Kanban rows where `Status == Inbox` only**. Daily-note inbox section is not in scope — emergent work flows through `/capture` (silent listen + handover batch). This skill exists for tasks user dumps directly into Notion (mobile, desktop, meeting-on-the-fly).

## What to do

1. **Query Notion Kanban** for rows where `Status == Inbox`. Use `mcp__claude_ai_Notion__notion-query-data-sources`. DB + data source IDs in `CONTEXT.md`.

2. **For each row**, propose ONE disposition:
   - **Set Type + optional Due + Status move** (Inbox → Today, or keep Inbox). Type ∈ `delivery / code / learn / admin / people / decision`. Set Project slug if implied (must match an existing `vault/initiatives/<slug>.md` or `_unassigned_`).
   - **Discard** — set `Status=Done`, prepend `[discarded] ` to title (MCP does not expose archive/trash). User can true-trash in Notion UI later.

3. **Present batch as table** before acting:

   ```
   # | Title                          | Proposed                              | Why
   1 | "call Anna re Q3 OKRs"         | Type=people, move to Today            | Implied this week
   2 | "read book X"                  | Type=learn, keep Inbox                | No urgency signaled
   3 | "fix typo in spec"             | Type=delivery, Due=today, move Today  | Trivial, do today
   4 | "leftover from old project"    | Discard                               | No active initiative ties to it
   ```

4. **Wait for user confirmation** ("yes", "skip 2", "change 3 to discard"). Apply in one batch after confirmation. No partial without telling.

5. **After apply**, summary: N updated, N discarded. Caveman.

## Hand-offs

- If a row is actually a decision needing thought (not a task): suggest `/spar` instead of triaging.
- If a row needs polished prose for a destination: suggest `/draft` after triage.
- Rows that mention an initiative/domain/system not yet in vault → flag for user. Don't auto-create; new nodes happen via `/capture` handover flow.

## Hard rules

- Do not duplicate items from team boards (Primary/DEV/Product). Surface match if found, ask user.
- Do not invent due dates — only set if implied in row text.
- Do not move items past `Today` (no auto-`Doing`). User picks `Doing`.
- Do not write to vault. Vault writes happen only via `/capture` handover flow.
- If Notion MCP not authenticated, ask user to run `mcp__claude_ai_Notion__authenticate`.

## Caveman default

Output terse. Table + confirm + summary. No filler.
