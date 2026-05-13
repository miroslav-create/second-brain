---
type: concept
tags:
  - concept/product-type
aliases:
  - mutual_fund
  - Podílový fond
  - Mutual fund
---

# ProductType — Mutual fund

> Czech `podílový fond`. Actively or passively managed open-end fund sold by an investment house or bank distributor. Headline = `management_pct` (p.a.) + `entry_fee`. Carries [[Performance]] entries.

[[Overlap]]: **adjacent** — substitute for funds held outside [[Provider - Fondee|Fondee]].

## Headline shape

- `management_pct` — often `None` because real per-fund fees sit in KIID PDFs, not HTML (see [[Known issues]] § per-fund mgmt fees behind KIID PDFs)
- `entry_fee` — free-text (typical "max 3 %")
- `note` — `management_note` carries trustworthy context when numeric extraction misses
- [[Performance]] list — 1y / 12m / 3y / 5y windows

## Providers emitting this type (5)

- [[Provider - JT IS]] — 3 funds (Money / Dividend / Bond) with 1y perf
- [[Provider - Conseq]] — ~14 funds from Active Invest + Conseq Invest tiers; NAV-based extraction
- [[Provider - Generali]] — 3 premium + 6 listed funds; 12m / 3y / 5y perf
- [[Provider - KB]] — Buy and Watch fund (alongside savings + DIP)
- [[Provider - ČSOB]] — bank-distributed funds (limited extraction)

## "Portfolio" disambiguation

This type is **not** "portfolio". A bank's mutual-fund product is a `mutual_fund`, even if the bank markets it as "investment portfolio". The word **portfolio** is reserved for [[ProductType - Robo portfolio]] (Czech "robo portfolio") in this domain.

## Relationships

- Parent: [[ProductType]]
- Belongs to a [[Product]] via `product_type=MUTUAL_FUND`
- Wrapped in [[Overlap]] = adjacent
- Has zero-or-more [[Performance]] entries
- May be held inside a [[ProductType - DIP]] wrapper

## See also

- [[About]] § Product types
- [[Status]] § Investment houses
- [[ADR-0003 product-centric taxonomy]]
- `CONTEXT.md` § ProductType
