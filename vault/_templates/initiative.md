<%*
const title = await tp.system.prompt("Initiative slug (kebab-case)");
await tp.file.rename(title);
await tp.file.move(`/initiatives/${title}`);
-%>
---
type: initiative
slug: <% tp.file.title %>
status: active
started: <% tp.date.now("YYYY-MM-DD") %>
domain:
systems:
stakeholders:
hat:
---

# <% tp.file.title %>

## Why
<!-- What problem / outcome. One paragraph. -->


## Scope
<!-- In / out. Bound it. -->


## Open questions
- 

## Decisions
<!-- Date-stamped one-liners. Append-only. CC appends here on handover. -->


## Change log
<!-- Date-stamped what-changed. CC appends here on handover. -->


## Links
<!-- [[domain]], [[system]], [[meetings]], external URLs. -->

