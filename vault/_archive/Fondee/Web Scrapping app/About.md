# [[Name|Fund Snoop]] — About

## Use case

See what competition is offering across **all investment-adjacent products** — [[ProductType - Robo portfolio|robo portfolios]], [[ProductType - Savings account|savings accounts]], [[ProductType - Term deposit|term deposits]], [[ProductType - ETF headline|headline ETFs]], [[ProductType - Mutual fund|mutual funds]], [[ProductType - DIP|DIP]] — and compare to [[Provider - Fondee|Fondee]]'s pricing.

On-demand scraper (CLI or button-press web launcher). Pulls competitor fees / rates / products / minimums / performance, renders a faceted-sidebar static HTML dashboard. Non-technical colleagues view via OneDrive shared link or run the launcher locally.

## Scope

### Providers wired (11) + Phase E backlog (~13)

**Wired:**
- **Robos**: [[Provider - Fondee]], [[Provider - Portu]], [[Provider - Indigo]]
- **Banks (mutual funds / robo portfolios)**: [[Provider - KB|KB Investice]], [[Provider - ČSOB]], [[Provider - Erste ČS|Erste / ČS]]
- **Brokers**: [[Provider - Trading212]], [[Provider - XTB]], [[Provider - eToro]], [[Provider - Revolut]], [[Provider - DEGIRO]]

**Phase E backlog:**
- **Savings-account banks**: [[Provider - Air Bank]], [[Provider - Creditas]], [[Provider - Raiffeisen]], [[Provider - Trinity]], [[Provider - Max banka]], [[Provider - mBank]], [[Provider - Moneta]], [[Provider - UniCredit]]
- **Investment houses**: [[Provider - JT IS|J&T Investiční společnost]], [[Provider - Conseq]], [[Provider - Generali|Generali Investments]]
- Plus Phase D leftover fixes for [[Provider - Indigo|Indigo]] / [[Provider - KB|KB]] / [[Provider - ČSOB|ČSOB]] / [[Provider - Erste ČS|Erste]] / [[Provider - Fondee|Fondee]]

### Product types ([[ADR-0003 product-centric taxonomy|ADR-0003]])

[[ProductType - Savings account|savings_account]] | [[ProductType - Term deposit|term_deposit]] | [[ProductType - Robo portfolio|robo_portfolio]] | [[ProductType - ETF headline|etf_headline]] | [[ProductType - Mutual fund|mutual_fund]] | [[ProductType - DIP|dip]]. Single product-centric [[Product]] dataclass; each [[Provider]] has zero-or-more Products. Dashboard rows = Products, filterable by [[ProductType]] + Provider + headline %.

### Fields per Product

- Headline % (`management_pct` for fee-bearing, `rate_pct` for rate-bearing) — see [[Product]]
- `rate_ceiling` / `rate_conditions` (savings accounts)
- `term_months` (term deposits)
- `ticker` / `isin` (ETFs)
- `minimum_first` / `minimum_recurring` — see [[Minimum]]
- `performance` list (managed Products only) — see [[Performance]]
- `note` (always — trustworthy plain-text context when numeric extraction fails)

### Out of scope

- Full broker instrument catalogues (10k+ rows each → [[ADR-0002 broker scope|ADR-0002]])
- Scheduled / automated runs (on-demand only → [[ADR-0001 on-demand only|ADR-0001]])
- Hosted dashboard (Power BI / Streamlit) — static HTML only
- Internal Fondee data feed — Fondee scraped like any other provider

## Phase plan

- **A — Skeleton + 3 robos** ✅ (Fondee, Portu, Indigo)
- **B — Banks** 🟡 scaffolded — Phase D / E selector rewrite pending
- **C — Brokers** 🟡 scaffolded — headline ETFs hardcoded per ADR-0002
- **D — Per-site CSS selectors** 🟢 Portu shipped 2026-05-10; Erste / Indigo / KB / ČSOB outstanding
- **E — Product-centric dashboard + new providers** 🟢 foundations shipped 2026-05-10 (schema + faceted dashboard + 48 tests). Outstanding: 13 new providers (savings accounts + investment houses).

## Repo

`c:\Users\miroslav.zachar\OneDrive - Direct\Projects\Scrapping\`

Key docs in repo:
- `README.md` — quick run
- `HANDOVER.md` — current phase status + known issues + 13-provider backlog
- `CONTEXT.md` — domain language ([[Provider]], [[Product]], [[ProductType]], [[FeeSchedule]]…)
- `docs/adr/` — architectural decisions ([[ADR-0001 on-demand only|0001]] on-demand, [[ADR-0002 broker scope|0002]] broker scope, [[ADR-0003 product-centric taxonomy|0003]] product taxonomy, [[ADR-0004 summary as landing|0004]] summary landing)

## Decisions

- Name → [[Name|Fund Snoop]]
- Tech → [[Tech stack]]
- Output → static HTML on OneDrive (`Scrapping/reports/index.html`)
- Cadence → on-demand (manual CLI run)

## How it works

Diagrams (mind map + data flow + scraper anatomy + renderer split) in [[Mind map]].
