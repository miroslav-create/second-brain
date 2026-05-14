# second-brain — quick reference

```
sit down → open Claude Code → work + chat freely → "prepare handover" → done
```

## What happens behind the scenes

```
   you say:  "switched Snowflake to X-Small for cost"
                    │
                    ▼
   CC stays silent, marks it as a capture candidate
                    │
                    ▼
   you say:  "prepare handover"
                    │
                    ▼
   CC:  [[systems/snowflake]] — "switched to X-Small for cost"
        approve? (y/e/s)
                    │
                    ▼
   you approve  →  CC writes date-stamped line to snowflake.md
```

## The three tools

| Notion       | Tasks. Inbox → Today → Doing → Done.        |
| Obsidian     | Living docs + graph (change-impact view).   |
| Claude Code  | Partner + silent scribe.                    |

## The three node folders in `vault/`

| `initiatives/` | Named multi-week effort   | `pis-migration.md`         |
| `domains/`     | Standing business area    | `payments.md`              |
| `systems/`     | App / API / vendor        | `stripe.md`, `snowflake.md`|

Cap at three. Push back before adding a fourth.

## Where the details live

- Capture rules (verbs, trigger phrases, format) → [`.claude/skills/capture/SKILL.md`](.claude/skills/capture/SKILL.md)
- Daily ops, skills, conventions → [`HOW-TO-USE.md`](HOW-TO-USE.md)
- System state for next CC session → [`HANDOVER.md`](HANDOVER.md)
