---
type: concept
tags:
  - concept/domain
aliases:
  - fee schedule
  - poplatky
---

# FeeSchedule

> Provider-wide fee context: management %, transaction fee, FX spread, withdrawal/entry — captured at [[Provider]] level when the same schedule applies across the Provider's [[Product|Products]].

## Role in the domain

A FeeSchedule belongs to one Provider and describes fees that aren't per-Product. Per-Product fees (e.g. fund-specific management %) live on the Product itself.

## Fields

- `management_pct` — Provider-wide flat % p.a. when applicable
- `management_note` — trustworthy plain-text context (always set, even when numeric extraction fails)
- `transaction_fee`, `fx_fee`, `entry_fee`, `withdrawal_fee` — free-text strings
- `raw_notes` — diagnostic list (e.g. "all percents on page: [...]", WAF fallback markers)

## Relationships

- One [[Provider]] → one FeeSchedule
- Per-Product fees override FeeSchedule for that Product (see [[Product]] `entry_fee`, `transaction_fee`)
- [[Module - render report]] reads `management_pct` for the dashboard "fee" column

## Used by

- [[Module - schema]] — `FeeSchedule` dataclass
- [[Module - render report]] — flattens into [[Summary|SummaryCard]] fee mini-row
- All 22 scrapers fill it (some only `management_note`)

## See also

- [[About]] § Fields per Product
- [[Known issues]] § per-fund mgmt fees behind KIID PDFs
- `CONTEXT.md` § FeeSchedule

## Source of truth

`CONTEXT.md` + `fund_snoop/schema.py` (repo).
