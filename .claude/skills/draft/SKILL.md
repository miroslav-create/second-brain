---
name: draft
description: Turn messy bullets/notes into polished prose for a specific destination (Notion page, Slack/email message, spec section). User edits final. Invoke when user types /draft.
---

# /draft — bullets → prose

## What to do

1. **Ask for destination** if not specified. Required: pick exactly one.
   - `notion-page` — long-form, structured with headings, ready to paste into a Notion page.
   - `notion-task-description` — short, one-paragraph body for a Kanban row.
   - `slack` — conversational, ≤3 short paragraphs, no headings, em-dash OK.
   - `email-internal` — colleague tone, brief subject + body, no fluff.
   - `email-external` — formal-ish tone (clients, partners). Czech or English — ask.
   - `spec-section` — BA/PO output. Structured with intent/scope/AC/open-questions where appropriate.

2. **Read the input.** Source is either the user's pasted bullets/notes in chat, or a path they specify in vault (e.g., `vault/projects/foo.md`). Read it.

3. **Ask 1–2 sharpening questions max** if scope unclear:
   - Audience? (single name, team, public)
   - Tone? (matter-of-fact, persuasive, apologetic, celebratory)
   - Length cap? (default: as short as possible while complete)
   - Language? (Czech or English — default English unless input is Czech)

4. **Produce the draft** in a single code block / markdown block, ready to copy. Do NOT preface with "Here's the draft" — just output the draft.

5. **After draft, offer 2 alternative one-line headlines / subject lines** if applicable.

6. **Wait for user edits.** If user says "tighter" / "longer" / "more direct" / specific change — revise and re-emit. Do not save anywhere automatically.

## Style rules for the draft itself

- Skip filler ("just", "really", "actually", "I hope this finds you well").
- Lead with the point. Reason after, not before.
- No invented facts. If a fact is missing, mark `[TODO: confirm X]` instead of making it up.
- Match the input's specificity — don't add detail not in the source.

## Caveman boundary

- Your **process talk** (questions to user, status updates) = caveman.
- The **draft itself** = normal prose for the destination's audience. Caveman does not bleed into output.
