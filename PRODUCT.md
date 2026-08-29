# Product — Situational Awareness: A Post-Mortem

## What it is
A multi-page, static, self-contained research website documenting how the hedge fund
**Situational Awareness** (Leopold Aschenbrenner, CIK 0002045724) grew from ~$255M to a
~$45B fund in five quarters — and lost the bulk of it in July 2026 when its leveraged,
concentrated AI/memory book met a momentum reversal, was sold to Citadel, and the
original book was effectively destroyed (legal shell still active; filings continue).

## Audience & scene
Financially literate readers wanting the definitive, evidence-anchored account:
journalists, students, fund professionals, investors who watched the tape. They read on
desktop in a quiet room, or on a phone at a desk. No action to take — comprehension is
the success criterion (Read mode).

## Product truth (facts, not styling)
- Every number must trace to SEC EDGAR (13F-HR series for CIK 0002045724) or a named
  news outlet with the byline/date. Unverifiable items are labeled UNCONFIRMED.
- Framing decision (user-locked): **honest post-mortem** — "the original $45B fund is
  gone; the legal shell still files." Never claim the entity dissolved.
- AUM arc (13F gross public book): $254.8M (2024-12-31) → $1.01B → $2.12B → $4.14B →
  $5.52B (2025-12-31) → $13.68B (2026-03-31) → $20.24B (2026-06-30, 26 positions).
- Key holdings: memory/silicon (SanDisk $5.67B, Micron $5.57B at 2026-06-30),
  neoclouds/power (CoreWeave, Nebius, Core Scientific, Bloom Energy, Applied Digital),
  big put hedges on NVDA/ORCL/AVGO/AMD and a VanEck Semiconductor ETF put.
- July 2026: ~67% equity loss in July (press); public book sold to Citadel
  (Reuters, Jul 30 2026); Griffin letter: >80% of acquired risk unwound within 3 weeks
  (Aug 21 2026); fund invested $400M in Source Foundry after the rout (press);
  still held Anthropic private shares; 13G/A filed 2026-08-14 (SharonAI).
- Structure: SAF AI GP LP → Situational Awareness LP (adviser) → Partners LP /
  Offshore LP / Nov 2024 Series (Form D vehicles). Co-PM: Carl Shulman (per EDGAR
  cover pages — label "reported" where sourced).
- Manager: Leopold Aschenbrenner, 24, former OpenAI researcher, 18-month AI plan author,
  "Nostradamus of AI" (AsiaOne).
- Site scope (user-locked): multi-page — index, story, numbers, record, sources.
- Visual world (user-locked, fusion): forensic data-room (near-black, mono accents,
  incident-report grammar) + FT-style editorial (serif display, chart-driven restraint)
  + brutalist crash-report numerals (oversized figures, stark blocks, signal red for
  loss semantics).
- No build step. Pure HTML/CSS/SVG/JS, opens by double-click. No external font CDNs
  required at runtime (system stacks or bundled fonts — prefer system stacks + one
  locally referenced mono/serif where the OS has them).

## Constraints
- Files: index.html, story.html, numbers.html, record.html, sources.html,
  assets/ (css, js). evidence.md dossier at root.
- Detector must be run once at finish (detect.mjs). Finish reviewer + documenter
  subagents run at the end.
- Desktop + mobile screenshots into .impeccable/review/.
