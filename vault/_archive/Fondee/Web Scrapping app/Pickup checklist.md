# [[Name|Fund Snoop]] — Pickup checklist

Last updated: **2026-05-11**

Use when resuming after a break. Source of truth: `HANDOVER.md` in repo. Cross-ref [[Status]], [[Known issues]], [[Mind map]].

## Before touching code

1. `cd "c:\Users\miroslav.zachar\OneDrive - Direct\Projects\Scrapping"`
2. Read `HANDOVER.md` — live phase + open backlog
3. Skim `CONTEXT.md` — domain language (Provider, Product, ProductType, Summary, Overlap…)
4. `pytest` → expect **89 / 89 pass**
5. `python -m fund_snoop --render-only` → expect `reports/index.html` + `reports/products.html` written

## Current state

Phase F shipped (summary landing + Fondee paint + ADR-0004). 22 providers wired, 70 products, 89 tests. Phase E backlog cleared 2026-05-10.

## Next ideas (not urgent, pick what's interesting)

### 1. Term-deposit scrapers (low effort, high coverage win)

Banks already wired (Air Bank, Creditas, Max banka, Trinity) advertise `term_deposit` rates on separate product pages. One probe → scraper per bank yields `Product(type=TERM_DEPOSIT, term_months=N, rate_pct=X)`. Pattern identical to savings scrapers.

### 2. Diff-vs-previous-snapshot view

`data/snapshots/snapshot-YYYY-MM-DD.json` archive already accumulates. Add "what changed since last run" column / section to summary page. Schema supports comparison — just needs renderer work.

### 3. Zero-install PyInstaller bundle

```powershell
pyinstaller --onefile `
  --add-data "fund_snoop/web/templates;fund_snoop/web/templates" `
  -n fund-snoop fund_snoop/web/__main__.py
```

Bundle Playwright Chromium separately or via `PLAYWRIGHT_BROWSERS_PATH`. ~150 MB exe. Unblocks non-Python Fondee staff.

### 4. KIID PDF extraction pass

`mutual_fund` Products lack `management_pct` because data lives in PDFs. If UI demands per-fund mgmt fee comparison, add a PDF parser. Defer until asked.

### 5. UniCredit WAF workaround

Try residential UA / stealth plugin / scrapingbee fallback. Only if accuracy matters more than the hardcoded 2,0 % SAVE fallback.

## Per-provider pattern (for new scrapers)

1. **Probe.** `python scripts/probe_generic.py <slug> <url>` → dumps body.txt + raw.html + summary.txt to `tests/fixtures/_probe/<slug>/`.
2. **Inspect dump.** Find element wrapping the headline rate / fee. Decide selector strategy.
3. **Write scraper.** Emit `Product` directly into `ProviderEntry.products`:
   ```python
   return ProviderEntry(
       name=self.name,
       provider_type=ProviderType.BANK,
       source_url=self.source_url,
       products=[
           Product(
               name="Termínovaný vklad 12M",
               product_type=ProductType.TERM_DEPOSIT,
               rate_pct=4.5,
               term_months=12,
           )
       ],
   )
   ```
4. **Register** in `fund_snoop/providers/__init__.py` `ALL_SCRAPERS`.
5. **Run web app.** Double-click `start_app.bat` → button → check live log. Open report.
6. **Real fixture.** Save element text into `tests/fixtures/<provider>.txt`. Add fixture-backed test in `tests/test_providers.py`.

## Reference scrapers (by pattern)

- **Headline savings rate (single tier)**: `airbank.py`, `raiffeisen.py`. Anchor a single phrase like `úroku X % ročně` or `až X % p. a.`.
- **Tiered savings (ceiling + over-tier)**: `maxbanka.py`, `moneta.py`. Anchor on tier label + nearest %.
- **Multi-product savings on one page**: `mbank.py`. Iterate per product name, capture nearest %.
- **WAF fallback**: `unicredit.py`. Detect challenge text, return hardcoded value flagged in raw_notes.
- **Mutual-fund tile parsing**: `jtis.py` (1 perf), `generali.py` (3 perfs).
- **Fund-list NAV table**: `conseq.py`. Multiline regex on `<name>\n<NAV> CZK`.
- **Homepage pivot for banks**: `kb.py`, `csob.py`. When deep fee pages are JS-walled, scrape headline product tiles from `/`.
- **Tab click + xpath panel**: `portu.py`. Fee panel behind tab, `Bez slevy X %` anchor.

## Run cheats

```powershell
python -m fund_snoop                 # full scrape + render
python -m fund_snoop --render-only   # re-render latest snapshot, no network
python -m fund_snoop.web             # boot launcher manually
pytest                               # 89 / 89
pytest tests/test_providers.py -v    # provider parse tests
python scripts/probe_generic.py <slug> <url>   # probe a new site
```

## Locked decisions (don't relitigate)

- **ADR-0001** — on-demand only, no scheduler.
- **ADR-0002** — brokers = headline ETFs only, not full instrument catalogue.
- **ADR-0003** — product-centric data model + shared `ProductType` taxonomy.
- **ADR-0004** — summary cards as landing page; faceted table at `/products`.
- Stack: Python ≥ 3.11, Playwright Chromium headless, Jinja2, FastAPI + uvicorn + SSE, vanilla JS.
- `note` field is the trustworthy plain-text context per Product; numeric fields are best-effort.
- Output paths: `reports/index.html` + `reports/products.html` + `reports/archive/*-YYYY-MM-DD.html` + `data/snapshots/snapshot-YYYY-MM-DD.json`.
- Distribution: static HTML on OneDrive (preferred over hosted dashboards — see decision in [[About]]).
