# [[Name|Fund Snoop]] — Known issues

Last updated: **2026-05-11** (post Phase F)

Cross-ref [[Status]], [[Pickup checklist]], [[Mind map]].

## Live state

22 / 22 [[Provider|providers]] ok, 0 errors, 70 synthesized [[Product|products]], 89 / 89 tests pass. All Phase D / E backlog items shipped. Remaining gaps are quality / coverage, not red bugs.

## Open items

### Term deposits not yet emitted

Banks already scraped ([[Provider - Air Bank|Air Bank]], [[Provider - Creditas|Creditas]], [[Provider - Max banka|Max banka]], [[Provider - Trinity|Trinity]]) advertise [[ProductType - Term deposit|`term_deposit`]] rates on adjacent product pages. Schema + dashboard ready. Need one probe → scraper per bank yielding `Product(type=TERM_DEPOSIT, term_months=N, rate_pct=X)`.

### Per-fund mgmt fees behind KIID PDFs

[[ProductType - Mutual fund|`mutual_fund`]] Products from [[Provider - JT IS|J&T]] / [[Provider - Conseq|Conseq]] / [[Provider - Generali|Generali]] / [[Provider - KB|KB]] / [[Provider - ČSOB|ČSOB]] lack `management_pct` — real per-fund fee data sits in KIID PDFs, not HTML. Either accept `None` (`mutual_fund` rows render without fee badge) or add PDF-extraction pass. Deferred until UI demands it.

### UniCredit WAF fallback

BrightCloud challenge blocks headless Chromium. [[Provider - UniCredit|UniCredit]] scraper falls back to hardcoded 2,0 % SAVE, flagged in `raw_notes`. Residential UA or stealth plugin would unblock real extraction — only worth it if accuracy matters.

### Trinity Bank inner pages

[[Provider - Trinity|Trinity]] SPA-routes 404 on direct GET. Only homepage stat block exposes the rate. If their wording shifts ("Narozeninový spořicí účet" → something else), scraper silently breaks.

### Broker `headline_instruments` are metadata

[[ProductType - ETF headline|`headline_instruments`]] populated from hardcoded class constants — `_parse(text)` does not extract them (see [[Module - providers base]]). If a broker URL succeeds but page content has drifted, dashboard still shows the curated ETF list. Treat broker instrument lists as documentation, not scraped data ([[ADR-0002 broker scope|ADR-0002]] trade-off).

### No diff-vs-previous-snapshot

`data/snapshots/snapshot-YYYY-MM-DD.json` archive accumulates but renderer doesn't compare. Schema supports it; just not built.

### Cosmetic — shared % filter range

Dashboard percent column shows both `management_pct` (green when below Fondee baseline) and `rate_pct` (always green). One filter range covers both. Fine because savings rates (2-5 %) and mgmt fees (0,5-1,5 %) rarely overlap numerically. If a savings account at 0,9 % appeared, the green-vs-baseline logic would look weird.

### Real-DOM fixtures coverage

Only Portu has a real-DOM fixture (`tests/fixtures/portu.txt`). The other 21 providers test against synthetic body-text fixtures. Synthetic catches regex regressions, not DOM drift.

### Zero-install distribution

Web launcher needs Python + Playwright Chromium pre-installed. For non-tech Fondee staff who don't already have those, a PyInstaller bundle (~150 MB exe) is the next step.

## Closed in Phase E / F (2026-05-10)

- ✅ 8 savings banks wired ([[Provider - Air Bank|Air Bank]], [[Provider - Creditas|Creditas]], [[Provider - Raiffeisen|Raiffeisen]], [[Provider - Trinity|Trinity]], [[Provider - Max banka|Max banka]], [[Provider - mBank|mBank]], [[Provider - Moneta|Moneta]], [[Provider - UniCredit|UniCredit]])
- ✅ 3 investment houses wired ([[Provider - JT IS|J&T IS]], [[Provider - Conseq|Conseq]], [[Provider - Generali|Generali]])
- ✅ [[Provider - Erste ČS|Erste]] mgmt 8,47 misread → anchor on `poplatek za správu`, cap-discard >5 %
- ✅ [[Provider - Indigo|Indigo]] URL → `/pricing` not `/poplatky`
- ✅ [[Provider - KB|KB]] / [[Provider - ČSOB|ČSOB]] homepage pivot
- ✅ [[Provider - Fondee|Fondee]] portfolio names → migrated to [[Product]]/legacy synthesis
- ✅ [[Summary]] landing page (Phase F, [[ADR-0004 summary as landing|ADR-0004]])
- ✅ Fondee Foundation paint (`#004033` + `#C4DE00`, Inter)

## Mitigation

[[FeeSchedule|`management_note`]] / [[Product|`Product.note`]] is the trustworthy plain-text context, set on every scraper regardless of regex success. Numeric extraction (`management_pct` / `rate_pct`) is best-effort; the note carries human-readable truth.

## Source of truth

Repo `HANDOVER.md` is authoritative for live phase + open backlog.
