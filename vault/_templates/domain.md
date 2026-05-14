<%*
const title = await tp.system.prompt("Domain slug (kebab-case, e.g. payments)");
await tp.file.rename(title);
await tp.file.move(`/domains/${title}`);
-%>
---
type: domain
slug: <% tp.file.title %>
owner:
---

# <% tp.file.title %>

## Mission
<!-- What this domain is responsible for. One paragraph. -->


## Active initiatives
<!-- Backlinks panel will surface; list anchors here too. -->


## Systems in scope
<!-- [[system-a]], [[system-b]] -->


## Decisions / patterns
<!-- Cross-cutting calls that apply across initiatives in this domain. Date-stamped. -->


## Notes

