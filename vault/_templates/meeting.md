<%*
const topic = await tp.system.prompt("Meeting topic (short slug)");
const fname = `meeting-${tp.date.now("YYYY-MM-DD")}-${topic}`;
await tp.file.rename(fname);
await tp.file.move(`/meetings/${fname}`);
-%>
---
date: <% tp.date.now("YYYY-MM-DD") %>
type: meeting
tags: [meeting]
attendees: []
---

# <% tp.file.title %>

> Bullet outcomes/actions go to Notion Meetings DB. This note = long reasoning, context, what you actually thought.

## Pre-read / context


## What was said


## What I'm taking away
- 

## Open threads
- [[ ]]

## Links
- Notion meeting page: 
