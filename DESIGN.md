# DESIGN.md — Situational Awareness, a Post-Mortem

Records the shipped world as built (`assets/app.css`, `assets/app.js`, five pages). Not a plan.

**TOC** — [1 World](#1-world) · [2 Palette](#2-palette) · [3 Type](#3-type-system) · [4 Components](#4-surfaces--components) · [5 Motion](#5-motion) · [6 States & a11y](#6-states--a11y) · [7 Responsive](#7-responsive) · [8 Bans](#8-bans) · [9 File map & extension](#9-file-map--how-to-extend) · [10 Provenance](#10-provenance)

## 1. World

A hedge-fund autopsy rendered as a precisionist industrial plate: a forensic data-room (near-black ink ground, mono measurement accents) fused with FT-style editorial restraint (Charter-family serif prose, hard 1px rules, no ornament) and brutalist crash-report numerals (stenciled display-grotesk figures at 42–110px where the headline would normally go). The first viewport is three numeral fields separated by rule arrows — the numerals *are* the opening argument. Doctrine: **one raking light** — flat, unmodulated planes; depth comes only from a single plane-up (`--ink2`) and from 1px rules; **no soft shadows, no glow, no gradient**.

## 2. Palette

All values are custom properties in `app.css:5-15`.

| Role | Hex | Where used | Discipline |
|---|---|---|---|
| ink (ground) | `#0B0C0E` | page bg, filestrip, planes below `lit` | never tinted |
| ink2 (one plane up) | `#14161B` | `.band.lit`, bars, `.verdict`, conc-track | the *only* elevation — flat fill, no shadow |
| ink3 | `#101216` | alt plate | spare |
| paper (display) | `#E8E4D9` | all display type, `::selection` bg, focus ring, memory seg | the "light" of the plate |
| body | `#A09A8C` | running prose, table cells | 6.98:1 on ink |
| dim | `#6E7683` | mono labels, captions, cite, spine dots | measurement scale, never prose |
| **lose (signal red)** | `#D6492F` | SOLD/−67% numeral, `stamp`, `.lose` cells, hot spine dots, loss ticks | **reserved for loss/decreased/unverified semantics: SOLD, breach, −, unconfirmed** — never decorative, never emphasis |
| gain (slate state hue) | `#9AA3B0` | `.ok` (ACTIVE, VERIFIED), bar hover-ink family | a slate *within* the palette, not a new hue; the "confirmed/active" state |
| rule / rule2 | `#242832` / `#3A4152` | 1px hard rules; lifted rule = focus of attention | only two rule weights |

Shipped red-uses beyond pure loss (resolved in finish review): `sources.html` uses red for **Unconfirmed**, and `pagenav .here` uses red for the current-page marker. Both are "absence/negative" semantics — the red rule extends to *loss + unconfirmed + de-registered-from-facts*, not to mere emphasis.

## 3. Type System

Stacks (`app.css:17-19`):

- **display** — `"Arial Narrow", "Avenir Next Condensed", "Roboto Condensed", "Helvetica Neue", sans-serif`, weight 600 — h1/h2/h3, numeral fields, `.cv2`, `.pull` drop caps, pagenav labels, comp-row names.
- **serif** — `"Charter", "Iowan Old Style", "Palatino", "Book Antiqua", Georgia, serif` — body (17px/1.68), leads, pull quotes, claims. The *prose* face; mono is never used for prose.
- **mono** — `"SF Mono", ui-monospace, "Menlo", "Cascadia Mono", "IBM Plex Mono", monospace` — labels, data, nav, measurement, stamps. Uppercase, tracked 0.10–0.24em.

Scale anchors (clamped ranges, from the CSS):

| Role | Size |
|---|---|
| `.barcell .bl` | 9.5px mono |
| labels (klabel, cite, `.l`, conf, cl, colophon, th) | 10–10.5px mono, caps |
| `.num .s`, `.dtable` mobile | 10–11.5px mono |
| data (conc `.cv`, src-row `.who`, dtable td) | 11–12.5px mono |
| prose (body, src-row claim) | 15.5–17px serif |
| lead | 19–24px serif |
| h3 | 17–22px disp, +0.06em, caps |
| h2 | 30–64px disp, −0.01em, caps |
| h1 | 44–128px disp, −0.015em, caps |
| `.num .v` (hero numerals) | 42–110px disp, −0.02em, tabular-nums |
| `.quarter-readout .big` | 44–96px disp, tabular-nums |
| `.pull` | 22–34px serif italic |

Rules: `font-variant-numeric: tabular-nums` on every data readout (`.v`, dtable, `.cv`, `.vv`, `.ta`, `.big`); caps on all display and all mono labels; `mark` = paper + 600 (no background); `em` = paper italic; drop cap on first prose paragraph only (`.prose p:first-of-type::first-letter`).

## 4. Surfaces & Components

- **`.filestrip`** — sticky case header, top of every page; mono 10.5px cells, `role="banner"`; states: `.flag` (red status), `.ok` (slate), `.hide-sm` drops cells ≤760px.
- **`.band` / `.band.lit`** — the page spine; lit = one plane up; states: `.tight` (reduced padding), `.inner` (1480px cap).
- **`.nums` hero** — three stenciled numeral fields + `—→` rule arrows; states: `.lose` (red value + mixed label), `.stamp` (rotated bordered SOLD tag).
- **`.bars`** — 7 quarter bars, `data-h` set by JS; states: hover/`.active` brighten, `.peak` paper border; `.bv` value reveals on hover/active.
- **`.scrub` + `.tick`/`.flagline`** — the signature fixed-pitch scrubber (numbers page); states: `.tick.on`, `.flagline.lose` (red line), `.flagline.flip` (caption left-anchored for last cells).
- **`.dtable`** — mono 12.5px table, tabular-nums; states: `td.strong`, `td.big`, `.lose`, `.src`, row hover 3% paper, `th` rule2.
- **`.conc` rows** — name / 8px paper track / value; state: `.in` fills; hatched red = option position.
- **`.compose` + `.legend`** — peak-book composition strips (10px, `.seg` per theme) and the 5-key legend; `.seg.hedge` = red hatch for option rows.
- **`.spine`** — dated event rail, left rule + 8px nodes; state: `.ev.hot` red node (loss event only).
- **`.stamp` / `.stamp-line`** — rotated bounded mono tags; loss/terminal language only.
- **`.pull`** — serif italic quote, left rule, mono `.attr`.
- **`.sec30`** — index 4-cell summary grid (2×2 ≤900, 1-col ≤560); `.lose` inside `.cv2`.
- **`.verdict`** — ink2 plane, 2-col (label/stamp | finding); closes index, numbers, record.
- **`.pagenav`** — 5-link flex rule grid; state: `.here` paper + red idx + red bottom rule.
- **`.colophon`** — footer, mono 10px uppercase, wrap.

## 5. Motion

- **reveal** — `[data-reveal]` opacity 0→1, `translateY(26px)`→0, 0.7s `cubic-bezier(.16,.84,.24,1)`, via IntersectionObserver at 0.18 threshold; unobserved after firing.
- **bars** — `scaleY(0.001)`→1 on `.in`, 0.9s, stagger 0.06s × 7.
- **conc** — `scaleX(0.001)`→1, 1s, origin left, on `.in`.
- **scrubber step** — auto-plays through the 9 ticks at 140ms steps once, on first intersection (JS), then rests on the **last** tick (GONE) — the rail opens on the end state by design; `set(i)` is user-driven via click/arrow after.
- **flagline** — `left` transition 0.45s `--ease`; caption via `data-cap`.
- **reduced-motion** — CSS zeroes reveal/bar transitions; JS skips the auto-play, force-adds `.in` immediately.

## 6. States & a11y

- **focus-visible** — 2px paper outline, 4px offset, on a/button/tabindex (no default ring).
- **selection** — `background: var(--paper); color: var(--ink)`.
- **tablist/tab** — `#quarterScrub` is `role="tablist"`; ticks are `role="tab"`, `tabindex="0"`, `aria-label` with quarter + value; arrow keys step and move focus; `aria-selected` toggled in `set()`.
- **aria-live** — `qrAum` and `qrNote` are `aria-live="polite"` so scrub updates announce.
- **Landmarks** — `filestrip` = `banner`, one `main`, one `nav` (label "Site") per page; `bars` and scrub carry `aria-label`.
- **Contrast floor (measured)** — body on ink **6.98**, paper on ink2 **14.25**, red on ink **4.52**. dim `#6E7683` on ink is the dimmest shipped text (~3:1) — used only for 10px+ caps labels, never prose.

## 7. Responsive

| BP | Behavior |
|---|---|
| 900px | `.nums` → 1-col stack (arrows gone, tops as rules); `.hold-grid`, `.quarter-readout`, `.verdict`, `.sec30` collapse to 2×2 or 1-col |
| 820px | `.src-row` → 1-col, `.conf` left |
| 760px | `.filestrip` cells shrink to 9.5px, `.hide-sm` hidden; `.pagenav` links full-width |
| 720px | `.dtable` padding/font down, `.col-cut` columns drop; `.conc-row` → name spans full row |
| 600px | `.tick .ta` (in-track values) hidden; track `min-height:120px` |
| 560px | `.sec30` → 1-col |

**col-cut mechanism**: tables mark optional columns with `.col-cut`; @720px those cells unrender. `ta-label` (scrubber in-track value) hides ≤600px so the rail still reads via labels + readout.

## 8. Bans

Explicit refusals, enforced by the CSS and the pages:

- No soft shadows, zero `box-shadow` anywhere; zero `border-radius`; no glass, no gradient, no glow, no raking-light *shading* (planes are flat).
- No eyebrows (no over-typed kicker above a heading), no emoji, no unicode-as-icon glyphs, no gradient text, no mono-costume prose (mono is data/labels only — never running text).
- Red is not emphasis: it appears only at the 21 red-uses reserved for loss/decrease/unverified in the palette table.
- No CTA, no button-shape, no hover-scale, no `:visited` recolor beyond paper.
- No invented numerals: every figure carries a `cite`/`src` line and a confidence tag (verified / press / unconfirmed).

## 9. File Map & How to Extend

```
index.html     SA-001  case at a glance   (nums hero, bars, sec30, verdict)
story.html     SA-002  quarter story       (5 lit/plain bands, spine, pulls)
numbers.html   SA-003  scrub + composition (scrub, hold-grid, conc, compose)
record.html    SA-004  filings index        (dtable exhibits A–D, verdict)
sources.html   SA-005  claim register       (src-row × 13, 3-level legend)
assets/app.css shared world (tokens, components above)
assets/app.js  reveal IO + scrubber + bar data-h
PRODUCT.md     request + contract seed bf72df90
evidence.md    sourcing notes
.impeccable/review/   6 finish-review captures (desktop + mobile)
.impeccable/qb/        donor references (reference material only)
```

**New-page recipe.** Copy a peer, then: `filestrip` (banner, keep `hide-sm` cells) → `main` of `band`s (alternate `lit` for plane cadence) → `pagenav` (5 links, `.here` on self) → `colophon` (SA-00N tag). Reuse `.hold-grid` for two-ups, `.dtable` for any tabular data, `.klabel` before each section. Every new number must carry a `cite` line and one of the three confidence tags per `sources.html` (verified / press-sourced / unconfirmed); unconfirmed figures go to the register, never into `.nums`, `.big`, or the AUM arithmetic.

## 10. Provenance

- **No shipped raster assets.** The favicon is an inline `data:image/svg+xml` per page (identical 3-bar mark: two paper, one red).
- **Finish-review captures** live at `.impeccable/review/` — `desktop-index.png`, `desktop-story.png`, `desktop-numbers.png`, `desktop-record.png`, `desktop-sources.png`, `mobile-index.png`. Not linked from the site.
- **Donor reference** `.impeccable/qb/precisionist.webp` is the quality-bar donor from the impeccable.style world card; `tensegrity.webp` and `precisionist-hero.webp` sit alongside as named-raise references. Reference material only — none is shipped in the site or linked from a page.
