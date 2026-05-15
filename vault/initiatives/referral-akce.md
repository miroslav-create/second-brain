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

## Pre-meeting Qs

### Pre Martina (CTO)

- **Arch boundary**: `referral-batch-service` standalone alebo modul existujúceho monolitu? Trade-off: deployment isolation vs prevádzková réžia.
- **Event source**: 5 trigger eventov (assign/register/contract/deposit/reward) — existujúci event bus alebo polling DB state? Ak bus, retry semantics + dead letter strategy?
- **Idempotency design (Q5)**: DB unique constraint `(referral_id, batch_run_id)` postačí, alebo transactional outbox pattern pre exactly-once payout?
- **Storno integrácia**: full account storno → re-validate. Webhook z KYC/účet systému, alebo nightly reconciliation? SLA pre stale state?
- **Privacy column**: `referral.display_name_revealed` — column na referral table OK? Migration plan, default false, backfill.
- **Batch ops**: cron monitoring, alerting threshold, manual re-run mechanism ak job failne v stredu o 2:00.
- **Fadmin API contract**: REST endpoints na state history alebo direct DB read-only role pre Fadmin? Auth model.
- **Config strategy**: 500 CZK / 20 EUR / 100 PLN + min vklad 1000 Kč — runtime config (DB/feature flag) alebo deploy-time? Kto má právo meniť.
- **Rate limit / abuse**: validation endpoint open pre anonym? Bot guard ak nie?

### Pre Hanku (Data/BA lead)

- **Analytics scope v1**: len business eventy (assign/eligible/rewarded) alebo extended funnel (share kliky, share kanály)? Decision = E1/E2 instrumentation effort.
- **Baseline**: aktuálny referral funnel — máme data? Treba pre v2 conversion benchmarks.
- **Pilot success criteria**: 3-mesačný pilot — aké KPI definujú "ďalej" vs "stop"? Fraud pattern threshold (% suspicious / dispute rate)?
- **Reporting ownership**: dashboards real-time alebo daily batch? Kto owns post-launch? CC tím alebo growth tím?
- **Pseudonymisation**: v analytics layer `referral_id` raw alebo hash? GDPR audit pohľad.
- **Test dataset**: kto pripraví cross-currency edge cases, refund races, KYC-fail-after-complete scenarios? Bez toho QA blind.
- **AC template**: máme štandard pre user story acceptance criteria pre tieto epics? Konzistencia s ostatnými iniciatívami.
- **Handover BAU**: kto vlastní Doporučení po launchi? Robert manuálny batch — backup person ak PN/dovolenka?
- **Legal coordination (Q10)**: kto drives FAQ + PDF obsah deadline? BA, PO alebo Legal samé?

## Epic breakdown (návrh)

1. **E1 — Doporučení page (web, klient zóna)**: list view, share UI, infinite scroll, CZ texty. Závisí na E3+E4.
2. **E2 — Doporučení screen (mobile app)**: parita s E1. Native share.
3. **E3 — BE state machine + event ingest**: `IN_PROGRESS → ELIGIBLE → REWARDED`. 5 trigger eventov. State history audit log. Core blocker.
4. **E4 — Ref-code validation service**: strict reject FE + BE. Privacy: blocked = neexistujúci.
5. **E5 — Manuálny batch reward process**: Robert štvrtok job. Idempotency. Cross-currency payout.
6. **E6 — Fadmin CC observability integration**: state history API + reason flags. [[fadmin]] side.
7. **E7 — GDPR display-name reveal mechanism**: `display_name_revealed` bool. Reveal len ELIGIBLE/REWARDED. Reverz pri KYC-fail.
8. **E8 — Legal/Content delivery**: FAQ obsah, PDF podmienky, T&C copy. Non-tech blocker risk.

Alt: ak JIRA epic granularity tlačí menej epics → E1+E2 merge do "FE delivery" = 7 total.

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
- **2026-05-15** — Override CLAUDE.md hard rule *"Never duplicate team-board tickets. Link only."* pre tento initiative. Parent Project page (`[REF] Referral akce — projekt`) zapísaná do team `fromJIRA` DB napriek tomu, že nemá JIRA `Issue key`. Reasoning: 8 epikov už existuje v team DB s JIRA keys; native `Parent task` self-relation vyžaduje same-DB. Caveat: Ria's JIRA sync má neistú reakciu — orphan/purge/ignore unknown. Watch next sync run.
- **2026-05-15** — DDL: `Project` pridaný do `Tags` multi-select v data source `35e18e00-108d-816a-88e3-000baf8ced0a` (`ALTER COLUMN "Tags" SET MULTI_SELECT(...)`). Schema-drift voči JIRA sync source — JIRA túto hodnotu nepozná. Issue Type field nechané ako text=`Project` (JIRA-native semantic: Project > Epic > Story hierarchy).

## Change log

- **2026-05-15** — BA sparring nad špec dokončený (13 otázok: 9 resolved, 3 blocker open, 1 backlog v2). Komentáre zanesené do špec dokumentu (anchors `[CC-spar Q1..Q13]`). Status: in-review → ready-for-dev po doriešení Q4/Q5/Q10.
- **2026-05-15** — Pridané pre-meeting Qs (Martin CTO + Hanka Data/BA) + epic breakdown návrh (8 epics, alt 7).
- **2026-05-15** — 8 epics + 48 tasks vytvorené v Notion `fromJIRA` DB. Všetky Status=Backlog. Tags: `Epic` + doménový tag (FrontEnd/Back End/Mobilní Appka/FADMIN). Naming: `[REF-E{n}]` epics, `[REF-E{n}.{m}]` tasks. Parent task linknuté.
- **2026-05-15** — Notion epics+tasks preložené do CZ (Task name + Description). Technické termíny ostali EN.
- **2026-05-15** — Vytvorená parent Project page v Notion `fromJIRA` DB: `[REF] Referral akce — projekt` (ID 182, URL https://www.notion.so/36118e00108d8123af5ed1cc7261ef18). Issue Type=Project, Status=Backlog. Body = Cíl + Figma + 8 epic links + 3 blockery + full špec paste. 8 epikov (E1–E8) → `Parent task` = parent (dual relation `Sub-task` server-side overené 8/8). DDL: Tag `Project` pridaný do `Tags` multi-select. Caveat: row nemá JIRA `Issue key` → watch budúci Ria sync.

## Links

- **Špec dokument:** `C:\Users\miroslav.zachar\OneDrive - Direct\Projects\Referal Akce\Referral specifikace.md`
- **Figma — komponenty + ukážky obrazoviek:** https://www.figma.com/design/9Z5ZZ6m0R4C3sp0UvrqGpV/?node-id=201-2230
- **Figma — re-klik flow (prihlásený klient):** https://www.figma.com/design/9Z5ZZ6m0R4C3sp0UvrqGpV/?node-id=415-10523
- **Domain:** [[growth]]
- **System (CC tool):** [[fadmin]]
- **Future systems** (vzniknú pri dev): klientska-zona, fondee-app, referral-batch-service
