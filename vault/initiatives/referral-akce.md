---
type: initiative
slug: referral-akce
status: in-review
started: 2026-05-15
domain: "[[growth]]"
systems:
  - "[[fadmin]]"
stakeholders:
  - PO
  - BE tech lead
  - Legal/Compliance
  - Robert (manuálny batch)
  - Fadmin owner
hat: BA
---

# Referral akce

## Why

Redesign referral programu Fondee. Nový režim: nepretržitý beh bez limitu počtu doporučených, odmena 500 Kč / 20 EUR / 100 PLN obom stranám pri splnení podmienok (registrácia + zmluva + min vklad 1000 Kč). Akcia je growth lever pre acquisition cez existujúcich klientov. Timeline asap, web + appka súbežne.

## Scope

**In:**
- Stránka Doporučení v klientskej zóne (web) + nová stránka v appke
- BE state machine `IN_PROGRESS → ELIGIBLE → REWARDED`
- Trigger eventy (5: code assigned / registration completed / contract signed / deposit received / reward batch processed)
- Validácia ref-kódu (strict reject na FE)
- Manuálny batch process 1× týždenne (Robert, štvrtok)
- Integrácia s Fadmin (CC support workflow)
- v1 lokalizácia: **len CZ** texty pre share

**Out (v2 backlog):**
- Multi-language share-texts (SK/EN/PL)
- Rozšírená analytika (per-share-kanál events, conversion funnel)
- Soft fraud monitoring (device fingerprint, IP tracking)
- Quick-jump v zozname pri >200 doporučeniach

## Open questions

- **Q4** — Read-only správanie ref-kódu v kroku N pri manuálnom (nie prefilled) vyplnení v kroku 0. Owner: PO + BA. Decision time: 5 min.
- **Q5** — Mechanizmus duplicity guardu v batch processe (návrh: DB unique constraint na `reward_transactions(referral_id, batch_run_id)` + idempotency-key per run). Owner: BE tech lead. Decision time: 1h.
- **Q10** — Owner + deadline pre FAQ obsah a PDF podmienky. Doriešiť v sprint plan meeting (BA + PO + Legal).
- **Q8 edge case** — Reverz mechanizmus `display_name_revealed = false` pri KYC-fail-after-completion. Doriešiť počas implementácie.

## Decisions

- **2026-05-15** — Currency cross-country: odmena sa pripisuje v mene účtu príjemcu (CZK/EUR/PLN), nie podľa krajiny doporučujúceho. Žiadna FX konverzia.
- **2026-05-15** — Validácia ref-kódu: strict reject na FE pred submit. Neexistujúci / blocked / archivovaný / self-referral → error "Kód neplatí". Privacy: blocked/archived sa správa rovnako ako neexistujúci.
- **2026-05-15** — Refund race: once ELIGIBLE, vyplať. Stav sa nevracia pri čiastočnom refunde. Re-validate len pri full storno účtu.
- **2026-05-15** — Pagination: infinite scroll, lazy load po 50, cursor-based. Sort `created_at DESC, id DESC`. Fallback "Load more" button pre a11y.
- **2026-05-15** — GDPR hard rule: `IN_PROGRESS` vždy zobrazí `"Nový uživatel"` (lokalizovane) bez ohľadu na zdroj mena (manuál / OAuth). Reveal len pri `ELIGIBLE`/`REWARDED`. BE pole `referral.display_name_revealed` (bool, default false).
- **2026-05-15** — i18n: v1 len CZ share-texty. SK/EN/PL → v2 backlog.
- **2026-05-15** — TTL pre IN_PROGRESS: žiadne. Marketingový tlak na dokončenie registrácie. Akceptované riziko: counter rastie monotónne.
- **2026-05-15** — Fraud guard: KYC + 1000 Kč min vklad postačujú v1. Pilot 3 mesiace, monitor patterns.
- **2026-05-15** — CC observability: cez [[fadmin]]. BE expose state history + reason flags v API/DB pre Fadmin integráciu.

## Change log

- **2026-05-15** — BA sparring nad špec dokončený (13 otázok: 9 resolved, 3 blocker open, 1 backlog v2). Komentáre zanesené do špec dokumentu (anchors `[CC-spar Q1..Q13]`). Status: in-review → ready-for-dev po doriešení Q4/Q5/Q10.

## Links

- **Špec dokument:** `C:\Users\miroslav.zachar\OneDrive - Direct\Projects\Referal Akce\Referral specifikace.md`
- **Figma — komponenty + ukážky obrazoviek:** https://www.figma.com/design/9Z5ZZ6m0R4C3sp0UvrqGpV/?node-id=201-2230
- **Figma — re-klik flow (prihlásený klient):** https://www.figma.com/design/9Z5ZZ6m0R4C3sp0UvrqGpV/?node-id=415-10523
- **Domain:** [[growth]]
- **System (CC tool):** [[fadmin]]
- **Future systems** (vzniknú pri dev): klientska-zona, fondee-app, referral-batch-service
