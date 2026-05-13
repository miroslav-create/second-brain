<%*
const name = await tp.system.prompt("Project name (slug)");
await tp.file.rename(name);
await tp.file.move(`/projects/${name}`);
-%>
---
created: <% tp.date.now("YYYY-MM-DD") %>
type: project
status: active     # active | paused | done | abandoned
tags: [project]
hat: ""            # BA | PO | PM | Delivery | AI-Amb | IT-Analyst
---

# <% tp.file.title %>

## Why this exists
<!-- The problem / motivation. -->


## Outcome
<!-- What 'done' looks like. -->


## Stakeholders
- [[ ]]

## Open questions
- 

## Decisions
- 

## Tasks
<!-- Atomic actions land in Notion private Kanban. Link the task there to here when worthwhile. -->


## Links
- Notion: 
- Figma: 
- Other:
