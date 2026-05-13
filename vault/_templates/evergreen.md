<%*
const title = await tp.system.prompt("Note title (concept, atomic idea)");
await tp.file.rename(title);
await tp.file.move(`/evergreen/${title}`);
-%>
---
created: <% tp.date.now("YYYY-MM-DD") %>
type: evergreen
tags: []
---

# <% tp.file.title %>

<!-- One atomic idea. Self-contained. Written in your own words. -->


## See also
- [[ ]]
