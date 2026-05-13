# [[Name|Fund Snoop]] — Status

Last updated: **2026-05-11**

## Current phase

**Phase F — Fondee paint + summary landing** — 🟢 shipped. `reports/index.html` is now a per-[[Provider]] card view (22 cards across 4 sections). Faceted table moved to `reports/products.html`. Fondee Foundation colors (`#004033` + `#C4DE00`) + Inter typography applied across both surfaces. [[ADR-0004 summary as landing|ADR-0004]] locked.

See [[Mind map]] for end-to-end picture.

## Working

- **22 / 22 [[Provider|providers]] ok, 0 errors** in live 2026-05-10 run, 70 synthesized [[Product|products]]
- **89 / 89 unit tests pass** (`pytest`)
- Repo scaffold, [[Module - schema|schema]], [[Module - runner|runner]], [[Module - render report|renderer]], [[Module - cli|CLI]], [[Module - web app|web launcher]] — all stable
- [[Summary]] landing: 22 [[Provider]] cards grouped Robo poradci / Banky / Brokeři / Investiční společnosti, ≤1 min reading time per card, [[Overlap|overlap badge]] (direct / adjacent / off) per [[Product]]
- Faceted dashboard at `/products`: [[ProductType]] + [[Provider]] checkboxes, % min/max range, sortable columns, green/red vs [[Provider - Fondee|Fondee]] baseline
- Self-contained HTML via Jinja `_fondee_tokens.css.j2` partial (single source of paint tokens)
- Web launcher (`start_app.bat` → button → SSE log → two report links) — see [[Module - web app]]
- Generic probe template (`scripts/probe_generic.py <slug> <url>`) used to scaffold 11 new providers in one session

## Providers wired (22)

- **Robos (3)**: [[Provider - Fondee|Fondee]], [[Provider - Portu|Portu]], [[Provider - Indigo|Indigo]]
- **Banks — mutual funds / DIP / robo (3)**: [[Provider - KB|KB]], [[Provider - ČSOB|ČSOB]], [[Provider - Erste ČS|Erste / ČS]]
- **Banks — savings accounts (8)**: [[Provider - Air Bank|Air Bank]], [[Provider - Creditas|Creditas]], [[Provider - Raiffeisen|Raiffeisen]], [[Provider - Trinity|Trinity]], [[Provider - Max banka|Max banka]], [[Provider - mBank|mBank]], [[Provider - Moneta|Moneta]], [[Provider - UniCredit|UniCredit]]
- **Brokers (5)**: [[Provider - Trading212|Trading212]], [[Provider - XTB|XTB]], [[Provider - eToro|eToro]], [[Provider - Revolut|Revolut]], [[Provider - DEGIRO|DEGIRO]]
- **Investment houses (3)**: [[Provider - JT IS|J&T IS]], [[Provider - Conseq|Conseq]], [[Provider - Generali|Generali Investments CEE]]

## What is new this session (Phase F)

- [[ADR-0004 summary as landing|ADR-0004]] `docs/adr/0004-summary-as-landing.md` — [[Summary|summary cards]] = landing page
- Renderer split (see [[Module - render report]]): `render_summary_html` / `render_products_html` / `render_html` (back-compat alias). New helpers `build_summary_cards`, `_overlap`, `_format_headline`, `_format_fee_line`, `_format_minimum`, `_top_products`. Czech section labels in `PROVIDER_TYPE_SECTION` (see [[ProviderType]]).
- New web route `GET /products` + repainted launcher index. SSE payload `render` now carries `summary_path` + `products_path`.
- 6 new render tests (overlap taxonomy, ProviderType grouping, 5-product cap, error-card, section anchors, Czech legend).
- Deleted: legacy `report.html.j2` (renamed to `products.html.j2`), unused `provider_card.html.j2`.

## Live data 2026-05-10 — verdicts

22 / 22 ok. Highlights:

| Provider | Headline | Verdict |
|---|---|---|
| [[Provider - Fondee\|Fondee]] | mgmt 0,9 ✅ | 2 portfolios; legacy schema |
| [[Provider - Portu\|Portu]] | mgmt 1,0 ✅ | Phase D xpath panel + `Bez slevy X %` |
| [[Provider - Indigo\|Indigo]] | mgmt 1,0 ✅ | flat 1 % from `/pricing` |
| [[Provider - KB\|KB]] | savings 3,5 ✅ + Buy and Watch 4,9 ✅ | homepage pivot |
| [[Provider - ČSOB\|ČSOB]] | savings 3,80 ✅ + DIP marker | homepage pivot |
| [[Provider - Erste ČS\|Erste / ČS]] | tx fee 0,35 % ✅ | mgmt suppressed; performance no longer misread |
| [[Provider - Air Bank\|Air Bank]] | savings 2,6 ✅ | ceiling + conditions |
| [[Provider - Creditas\|Creditas]] | savings 4,0 ✅ | base 3 + investing bonus |
| [[Provider - Raiffeisen\|Raiffeisen]] | savings 4,2 ✅ | from homepage |
| [[Provider - Trinity\|Trinity]] | savings 3,30 ✅ | from homepage stat |
| [[Provider - Max banka\|Max banka]] | savings 3,0 ✅ | tier break at 500k |
| [[Provider - mBank\|mBank]] | 4,01 / 5,01 / 3,01 ✅ | three savings products |
| [[Provider - Moneta\|Moneta]] | savings 2,6 ✅ | 1 mil. Kč ceiling |
| [[Provider - UniCredit\|UniCredit]] | savings 2,0 ✅ (fallback) | WAF block; hardcoded |
| [[Provider - JT IS\|J&T IS]] | 3 funds with 1y perf ✅ | 4,25 / 17,22 / 5,06 |
| [[Provider - Conseq\|Conseq]] | 14 funds + ZENIT DIP ✅ | NAV-based extraction |
| [[Provider - Generali\|Generali]] | 3 premium + 6 listed + DIP ✅ | 12m/3y/5y perf |
| 5 brokers ([[Provider - Trading212\|T212]] · [[Provider - XTB\|XTB]] · [[Provider - eToro\|eToro]] · [[Provider - Revolut\|Revolut]] · [[Provider - DEGIRO\|DEGIRO]]) | mgmt_note + headline ETFs | unchanged from Phase C |

[[Known issues]] for per-provider caveats. Detail in repo `HANDOVER.md`.

## Open backlog

- **[[ProductType - Term deposit|Term deposits]]** — not yet emitted. [[Provider - Air Bank|Air Bank]], [[Provider - Creditas|Creditas]], [[Provider - Max banka|Max banka]], [[Provider - Trinity|Trinity]] advertise term-deposit rates on adjacent pages. One probe → scraper per bank yields `Product(type=TERM_DEPOSIT, term_months=N, rate_pct=X)`.
- **Per-fund mgmt fees from KIID PDFs** — [[ProductType - Mutual fund|mutual_fund]] Products lack `management_pct` (data sits in PDFs, not HTML). Skip or add PDF-extraction pass.
- **UniCredit WAF workaround** — try residential UA or stealth plugin if accuracy matters.
- **Trinity inner pages** — SPA-routed, only homepage data exposed.
- **Diff-vs-previous-snapshot view** — schema supports it via `data/snapshots/` archive; not built.
- **Zero-install PyInstaller bundle** — for non-Python staff (~150 MB exe).

## Run

```powershell
cd "c:\Users\miroslav.zachar\OneDrive - Direct\Projects\Scrapping"
python -m fund_snoop               # full scrape + render
python -m fund_snoop --render-only # re-render latest snapshot
pytest                             # 89 / 89 should pass
# or double-click start_app.bat
```

## Source of truth

Repo `HANDOVER.md` is authoritative. This note mirrors it for Obsidian-side context.
