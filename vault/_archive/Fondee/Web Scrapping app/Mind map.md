# [[Name|Fund Snoop]] — Mind map

How the app works, end to end. See [[About]] for scope, [[Tech stack]] for stack rationale, [[Status]] for current phase.

## Conceptual mind map

```mermaid
mindmap
  root((Fund Snoop))
    Inputs
      ::icon(fa fa-globe)
      Competitor websites
        Robos: Fondee, Portu, Indigo
        Banks: KB, ČSOB, Erste, Air Bank, Creditas, Raiffeisen, Trinity, Max, mBank, Moneta, UniCredit
        Brokers: Trading212, XTB, eToro, Revolut, DEGIRO
        Investment houses: J&T IS, Conseq, Generali
    Triggers
      ::icon(fa fa-play)
      CLI: python -m fund_snoop
      Web launcher: start_app.bat → button
      Render-only: --render-only on cached snapshot
    Engine
      Playwright Chromium headless
      ProviderScraper per site
        scrape browser → text
        _parse text → Product list
      Runner orchestrates 22 scrapers
      Persist snapshot-YYYY-MM-DD.json
    Domain model
      Provider (ProviderType)
        robo
        bank
        broker
        investment_house
      Product (ProductType)
        savings_account
        term_deposit
        robo_portfolio
        etf_headline
        mutual_fund
        dip
      Overlap badge
        direct - robo_portfolio
        adjacent - savings/term/MF/DIP
        off - etf_headline
    Outputs
      ::icon(fa fa-file)
      reports/index.html - Summary cards 22 cards 4 sections
      reports/products.html - faceted table all Products
      reports/archive/*-YYYY-MM-DD.html
      data/snapshots/*.json
    Distribution
      OneDrive shared link
      Local FastAPI launcher SSE
      Future: PyInstaller exe
    Decisions
      ADR-0001 on-demand only
      ADR-0002 brokers = headline ETFs
      ADR-0003 product-centric taxonomy
      ADR-0004 summary as landing
```

## Data flow

```mermaid
flowchart TD
    U([User]) -->|double-click| BAT[start_app.bat]
    U -->|CLI| MAIN[python -m fund_snoop]
    BAT --> WEB[FastAPI /scrape]
    WEB -->|thread| RUN
    MAIN --> RUN[runner.run]

    RUN -->|for each scraper| PW[(Playwright Chromium)]
    PW -->|HTML/text| SCR[Provider scraper<br/>_parse]
    SCR -->|Product list| ENTRY[ProviderEntry]
    ENTRY --> SNAP[Snapshot]

    SNAP -->|JSON| DISK[(data/snapshots/<br/>snapshot-YYYY-MM-DD.json)]
    SNAP --> RND[render/report.py]

    RND -->|summary.html.j2| SUMM[reports/index.html<br/>22 Provider cards]
    RND -->|products.html.j2| TBL[reports/products.html<br/>faceted table]
    RND -->|archive| ARCH[reports/archive/<br/>*-YYYY-MM-DD.html]

    SUMM -.OneDrive link.-> NONTECH([Non-tech staff])
    TBL -.OneDrive link.-> NONTECH
    WEB -->|SSE events| BROWSER[Browser launcher UI<br/>live log + links]

    classDef store fill:#C4DE00,stroke:#004033,color:#004033
    classDef output fill:#004033,stroke:#004033,color:#fff
    class DISK,SNAP store
    class SUMM,TBL,ARCH output
```

## Scraper anatomy (one Provider)

```mermaid
flowchart LR
    URL[Provider URL] --> GOTO[base._get_body_text<br/>goto + wait + extract]
    GOTO --> TEXT[Body text or DOM panel]
    TEXT --> PARSE[_parse text]
    PARSE -->|regex / CSS selector| FIELDS[rate_pct<br/>management_pct<br/>rate_ceiling<br/>term_months<br/>etc.]
    FIELDS --> PROD[Product list]
    PROD --> ENTRY[ProviderEntry.products]

    PARSE -.fallback.-> RAW[raw_notes]
    PARSE -.WAF block.-> HARD[hardcoded value<br/>flagged in raw_notes]
```

Patterns: see [[About#Scope]]. Reference scrapers per pattern documented in repo `HANDOVER.md` § Pickup checklist.

## Renderer split (Phase F)

```mermaid
flowchart LR
    SNAP[Snapshot] --> BUILD_S[build_summary_cards]
    SNAP --> BUILD_P[build_product_rows]
    BUILD_S -->|SummaryCard list| TMPL_S[summary.html.j2]
    BUILD_P -->|ProductRow list| TMPL_P[products.html.j2]
    TMPL_S --> S[reports/index.html]
    TMPL_P --> P[reports/products.html]
    TOK[_fondee_tokens.css.j2<br/>colors + typography] -.include.-> TMPL_S
    TOK -.include.-> TMPL_P
```

## Cross-links

- [[Index]] — MOC for this folder
- [[About]] — use case + scope + phase plan
- [[Tech stack]] — Python + Playwright + Jinja2 + FastAPI rationale
- [[Status]] — current phase + live data verdicts (stale — repo `HANDOVER.md` authoritative)
- [[Known issues]] — per-provider scraper accuracy
- [[Pickup checklist]] — next-session tasks
- [[Name|Fund Snoop]] — name decision

### Concepts hub

- [[Provider]] · [[ProviderType]] · [[Product]] · [[ProductType]] · [[FeeSchedule]] · [[Minimum]] · [[Performance]] · [[Snapshot]] · [[Report]] · [[Summary]] · [[Overlap]]

### Decisions

- [[ADR-0001 on-demand only]] · [[ADR-0002 broker scope]] · [[ADR-0003 product-centric taxonomy]] · [[ADR-0004 summary as landing]]

### Modules

- [[Module - schema]] · [[Module - runner]] · [[Module - providers base]] · [[Module - render report]] · [[Module - web app]] · [[Module - cli]] · [[Module - paths]]

## Source of truth

Authoritative live state in repo: `c:\Users\miroslav.zachar\OneDrive - Direct\Projects\Scrapping\HANDOVER.md`. This map is logical; HANDOVER tracks Phase + open backlog.
