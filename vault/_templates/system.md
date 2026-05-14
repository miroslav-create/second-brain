<%*
const title = await tp.system.prompt("System slug (kebab-case, e.g. stripe)");
await tp.file.rename(title);
await tp.file.move(`/systems/${title}`);
-%>
---
type: system
slug: <% tp.file.title %>
owner:
contract:
last_touched: <% tp.date.now("YYYY-MM-DD") %>
domain:
---

# <% tp.file.title %>

## What
<!-- One paragraph: what this system does, where it lives, who runs it. -->


## Contract / interface
<!-- API shape, data flow, SLA, version. Pointers to external docs OK. -->


## Owners
<!-- Internal owner + vendor contact if applicable. -->


## Gotchas
<!-- Quirks, edge cases, things that bit us. Date-stamped if recent. -->


## Consumers
<!-- Auto via backlinks. Manually anchor known initiatives here too. -->


## Change log
<!-- Date-stamped: what changed, why, who. CC appends here on handover. -->

