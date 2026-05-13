---
type: module
tags:
  - module/core
aliases:
  - fund_snoop.providers.base
  - providers/base.py
  - ProviderScraper
---

# Module — `fund_snoop.providers.base`

> File: `fund_snoop/providers/base.py`. Scraper ABC + shared regex helpers. Every per-Provider scraper subclasses it.

## Purpose

Centralizes the Playwright load + body-text extraction logic so each scraper focuses on parsing. Splits each scraper into:

- **Network side** — `scrape(browser) -> ProviderEntry` — opens a `Page`, navigates, returns
- **Parse side** — `_parse(text) -> ProviderEntry` — pure function, offline-testable against text fixtures in `tests/fixtures/`

## Public surface

Class `ProviderScraper(ABC)`:

- attrs: `name`, `provider_type` ([[ProviderType]]), `source_url`
- `scrape(browser) -> ProviderEntry` *(abstract)*
- `_parse(text) -> ProviderEntry` *(default raises; subclasses override for text-based parsing)*
- `_new_page(browser)` — context with `cs-CZ` locale + desktop Chrome UA
- `_get_body_text(browser, url=None, *, wait_until, settle_ms, timeout)` — open url, settle, return body inner_text
- `_empty_entry(error)` — early-return helper for navigation failures
- `_scrape_via_text(browser, **kwargs)` — default flow combining `_get_body_text` + `_parse`

Module-level helpers:
- `PERCENT_RE`, `PERCENT_PA_RE`, `_DISCOUNT_PREFIX_RE` — compiled regex
- `first_percent(text)` — first `X %` value
- `first_percent_pa(text)` — first percent that has a `% p.a. | ročně | za rok` marker (skips discounts)
- `discount_percents(text)` — percents preceded by "sleva/akce/bonus/discount"
- `all_percents(text)` — all percents (diagnostic)

## Depends on

- [[Module - schema]] — `ProviderEntry`, `ProviderType`
- Playwright sync API

## Used by

All 22 per-Provider scrapers under `fund_snoop/providers/*.py`. Two patterns:

- `_scrape_via_text` default flow — 9 text-extraction scrapers (robos + most banks + brokers)
- Override `scrape()` directly — [[Provider - Portu]] (tab click + xpath panel), and any scraper with WAF fallback or SPA quirks

[[Module - runner]] iterates `ALL_SCRAPERS` and calls `.scrape(browser)`.

## Domain concepts handled

[[Provider]] (one instance per [[ProviderType]]) — does not own data, just produces it.

## Relevant decisions

- [[ADR-0001 on-demand only]] — synchronous Playwright loop, no async runtime
- [[ADR-0002 broker scope]] — broker subclasses populate `headline_instruments` from class constants, not via `_parse`

## Cross-links

- [[Mind map]] § Scraper anatomy
- [[Tech stack]] § Core
- [[Pickup checklist]] § Reference scrapers by pattern
- [[Known issues]] § per-fund mgmt fees behind KIID PDFs (limits of body-text parsing)

## Live state

Behavior + helper signatures may drift. Authoritative source: the file itself + `HANDOVER.md` § "Key files" and § "providers/base.py refactored".

## Source of truth

`fund_snoop/providers/base.py` (repo).
