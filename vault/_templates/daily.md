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

## Inbox
<!-- Drop any thought, task, idea, link here. `/triage` reads this section. -->
- 

## Doing today
<!-- Mirror Notion 'Today' column. Quick glance. -->
- 

## Notes / log
<!-- Reflections, decisions, observations. Long-form OK. -->


## Links touched
<!-- [[note-name]] entries built up during the day power graph view. -->


## Tomorrow
<!-- One-line nudge for future-you. -->
