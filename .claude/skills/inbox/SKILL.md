---
name: inbox
description: Drain Obsidian daily inbox + Notion private Kanban Inbox. Propose disposition per item (Notion task / evergreen note / discard). User confirms batch. Invoke when user types /inbox.
---

# /inbox — inbox sweep

## What to do

1. **Read today's Obsidian daily note**: `vault/daily/YYYY-MM-DD.md` (use today's date from environment context). Extract bullets under `## Inbox` heading. If file doesn't exist, ask user to open Obsidian and create today's daily note via Templater first — do NOT auto-create (Templater frontmatter matters).

2. **Read Notion private Kanban Inbox column**: query the private Kanban DB (ID in `CONTEXT.md` once created) for rows where `Status == Inbox`. Use `mcp__claude_ai_Notion__notion-query-data-sources` or equivalent.

3. **For each item across both sources**, propose ONE disposition:
   - **→ Notion task**: stays as Kanban row, set `Type` (delivery/code/learn/admin/people/decision), `Status` → `Today` or keep `Inbox`, set `Due` if implied. If item already in Notion, just enrich. If from Obsidian, create new Notion row + leave a `[x]` marker next to original bullet.
   - **→ Evergreen note**: extract into `vault/evergreen/<slug>.md` using `_templates/evergreen.md` shape. Propose `[[wikilinks]]` to existing notes (search vault first to surface candidates).
   - **→ Discard**: irrelevant or already obsolete. Leave bullet but strike through in Obsidian / archive Notion row.

4. **Present batch as a table** to user before acting:

   ```
   # | Source    | Item                            | Proposed  | Details
   1 | Obsidian  | "call Anna re Q3 OKRs"          | Notion    | Type=people, Due=this week
   2 | Notion    | "read book X"                   | Evergreen | new note evergreen/book-X-takeaways
   3 | Obsidian  | "fix typo in spec"              | Notion    | Type=delivery, Due=today
   ...
   ```

5. **Wait for user confirmation** ("yes", "skip 2", "change 3 to discard"). Apply only after confirmation. Apply in one batch — never partial without telling.

6. **After apply**, summarize: N created in Notion, N evergreen notes written, N discarded. Caveman.

## Hard rules

- Do not auto-create today's daily note. User opens Obsidian for that.
- Do not duplicate items already on team boards (Primary/DEV/Product). Surface match if found, ask user.
- Do not invent due dates — only set if implied in the item text.
- Do not move items past `Today` (no auto-`Doing`). User picks what's `Doing`.
- If Notion MCP not authenticated, ask user to run `mcp__claude_ai_Notion__authenticate`.

## Caveman default

This skill runs inside CLAUDE.md caveman context. Output terse. Table + confirmation prompt + summary. No filler.
