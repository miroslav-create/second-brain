# Tech stack — [[Name|Fund Snoop]]

[[About]]

## Core

- **Python ≥ 3.11** (using 3.14 locally)
- **Playwright (Chromium, headless)** — handles JS-rendered competitor sites + static pages with one tool
- **Jinja2** — HTML report templating
- **FastAPI + uvicorn + SSE** — local web launcher for non-tech users (`start_app.bat` → button → live log → report link)
- **Vanilla JS** — faceted-sidebar filtering + sortable table inside the static report (no framework, no bundler)

## Output

- Self-contained **static HTML dashboard** at `Scrapping/reports/index.html` (~40 KB, faceted sidebar) + dated archive in `Scrapping/reports/archive/`
- Raw JSON snapshots at `Scrapping/data/snapshots/snapshot-YYYY-MM-DD.json`
- No backend at rest, no database, no auth — share via OneDrive link
- Local FastAPI lives only while user runs `start_app.bat` (in-memory job state)

## Data model ([[ADR-0003 product-centric taxonomy|ADR-0003]])

- [[Provider]] ([[ProviderType]]: `robo | bank | broker | investment_house`)
- [[Product]] ([[ProductType]]: [[ProductType - Savings account|savings_account]] | [[ProductType - Term deposit|term_deposit]] | [[ProductType - Robo portfolio|robo_portfolio]] | [[ProductType - ETF headline|etf_headline]] | [[ProductType - Mutual fund|mutual_fund]] | [[ProductType - DIP|dip]]) — one row per offering in the dashboard
- Legacy `Portfolio` / `HeadlineInstrument` kept for back-compat (see [[Module - schema]]); `synthesized_products()` unifies all into a single Product list for the [[Module - render report|renderer]]

## Deps (pyproject.toml)

```toml
dependencies = [
    "playwright>=1.45",
    "jinja2>=3.1",
    "fastapi>=0.110",
    "uvicorn>=0.27",
]
[project.optional-dependencies]
dev = ["pytest>=8.0", "pytest-asyncio>=0.23"]
```

## Run

```powershell
python -m pip install -e ".[dev]"
python -m playwright install chromium
python -m fund_snoop
```

Entry: [[Module - cli]]. Web launcher: [[Module - web app]].

## Why this stack

- Python = canonical for scraping + data work; biggest ecosystem.
- Playwright = both static + JS sites, one API.
- Jinja2 = HTML output without JS frontend / backend.
- Static HTML on OneDrive = zero hosting cost, anyone with link can view.
- Vanilla JS in the report = filterable dashboard without a build step. Whole report stays one file.
- FastAPI launcher = button-press UX for non-tech staff without running a hosted service. Boots and dies on demand. See [[ADR-0001 on-demand only]] + [[Module - web app]].

## Rejected alternatives

- Node + Puppeteer — same capability, no team preference for JS.
- Python + requests + BeautifulSoup — would break on any JS-rendered competitor.
- Streamlit / Power BI / hosted web app — overkill for on-demand reporting; needs hosting + auth. (See [[Prefer static HTML on OneDrive over hosted dashboards]] decision.)
- React / Vue / build pipeline for the report — adds tooling churn for a static artifact; vanilla JS handles the filter UI in ~80 lines.
