<%*
const date = tp.date.now("YYYY-MM-DD");
const fname = date;
await tp.file.rename(fname);
await tp.file.move(`/daily/${fname}`);
-%>
---
date: <% tp.date.now("YYYY-MM-DD") %>
type: daily
tags: [daily]
---

# <% tp.date.now("YYYY-MM-DD, dddd") %>

## Notes / log
<!-- Free-form ephemera. Decisions, observations, scratchpad. Long-running prose belongs in initiative / domain / system notes — those persist; daily notes don't. -->


## Links touched
<!-- [[note-name]] entries built up during the day power graph view. -->


## Tomorrow
<!-- One-line nudge for future-you. Optional. -->
