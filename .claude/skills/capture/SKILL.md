---
name: capture
description: Listens through session for change-verb mentions tied to vault nodes (initiatives/domains/systems). Stays silent during work. Fires batched review when user says "prepare handover" or similar intent. User confirms appends; CC writes date-stamped one-liners to relevant notes. New nodes get classified + stubbed from template. Invoke automatically — main loop watches every user turn — and explicitly when user types /capture.
---

# /capture — living-docs capture flow

## Goal

Keep `vault/initiatives/`, `vault/domains/`, `vault/systems/` notes current without ceremony. Catch mentions during chat. Confirm + write at session end (handover).

## Three node types

- **Initiative** — `vault/initiatives/<slug>.md` — named multi-week effort.
- **Domain** — `vault/domains/<slug>.md` — standing business area (payments, ops, data-platform, people, etc.). Catch-all node.
- **System** — `vault/systems/<slug>.md` — app, API, vendor, integration. Receives backlinks from initiatives that touch it = change-impact view.

## When listening starts

Active **every user turn**. No invocation needed. `/capture` (explicit) forces the batched review now even if no handover intent detected.

## Detection rules

Mark a candidate when **both** appear in a single user utterance:

1. **Known node reference** — exact slug match against any filename in `vault/initiatives/`, `vault/domains/`, `vault/systems/`. Case-insensitive. Hyphen/underscore equivalent. Plain text mention counts (no `[[ ]]` required). Multi-word matches allowed if utterance contains all tokens of slug.
2. **Change-verb** — one of: `updated, changed, decided, switched, owner-is, started, stopped, added, removed, deferred, replaced, deprecated, migrated, scoped, blocked, unblocked, fixed, broke, learned, found, agreed`.

Also mark **new-node candidate** when:

- User mentions a proper-noun-looking entity (capitalized word or multi-word) not matching any existing slug, **alongside a change-verb**. Example: "started looking at Vendor Z" → `vendor-z` candidate, needs type classification.

## What CC does during session

- **Silent**. Never interrupt to confirm a single capture.
- Maintain in-memory list. Each entry: `{node_slug, node_type_or_unknown, trigger_quote_verbatim, timestamp}`.
- If user explicitly types `/capture`, fire the handover review immediately and clear list after.

## Handover trigger phrases

Fire review on these intents (user utterance contains substring, case-insensitive):

- "prepare handover"
- "wrap up"
- "end of session"
- "session summary"
- "for next session"
- "let's stop"
- "I'm done for today"
- explicit `/capture`

## Handover review format

```
N updates + M new nodes pending:

UPDATES
1. [[initiatives/pis-migration]] — "switched warehouse size to X-Small for cost"
2. [[systems/snowflake]] — "owner is now Eva"

NEW NODES (need classification: i=initiative / d=domain / s=system / skip)
3. vendor-z — "started integration scoping with Vendor Z"
4. tribal-sync — "agreed Tribal Sync runs Tuesdays"

approve all? edit individual (e:N)? skip individual (s:N)? classify new (3:s, 4:i)?
```

## Write actions on approval

For each approved **update**:

- Append to `## Change log` section of the target note:
  ```
  - <%date%> — <trigger_quote_verbatim>
  ```
- If target is initiative: also append to `## Decisions` if quote contains `decided/agreed/scoped`.
- For systems: bump `last_touched: <date>` in frontmatter.

For each approved **new node**:

- Resolve template by classification: `initiative.md` / `domain.md` / `system.md`.
- Render template (substitute slug + date manually since Templater is GUI-bound; CC writes plain markdown matching template body).
- Save to `vault/<type>s/<slug>.md`.
- Append `- <date> — <trigger_quote>` to `## Change log`.

For each **skipped**: drop silently. Do not re-surface in future sessions.

## Date format

`YYYY-MM-DD`. Use today's date from environment context, not invented.

## Hand-offs

- After handover review, if user wants polished prose for a captured item (e.g., expand a decision into a Notion page or email), suggest `/draft`.
- If a captured item is actually a decision needing more thought (user pushes back during review), suggest `/spar` to drill before writing.
- `/inbox` runs orthogonal — drains Notion Kanban Inbox column. Mention it if user asks "what about the tasks I dumped into Notion this morning?".

## Hard rules

- Never auto-write to vault during session. Always batch + confirm.
- Never invent change-log entries. Quote must be verbatim from user utterance.
- Never create a node without classification (initiative / domain / system). If user picks "skip", skip — do not coerce.
- Never overwrite existing sections. Always append.
- Trigger phrase detection runs on user message, not on assistant message. Do not self-trigger.
- If vault folders missing, abort with one-line error: "vault/initiatives|domains|systems folder missing. Run setup or create manually."

## Caveman default

Output terse per CLAUDE.md. Quotes preserved verbatim. List + confirm prompt + apply summary. No filler.
