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

**Notion Inbox count:** ___ <!-- thermometer: glance at Notion DB, write number. Threshold 15 (7-day avg) = add Friday skim ritual. -->

<!-- Drop any thought, task, idea, link here. `/inbox` reads this section. -->
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
