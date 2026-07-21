# CLAUDE CODE HANDOFF — Nike CMO Intelligence Dashboard

**You are building:** a single-file, self-contained HTML dashboard (`nike-cmo-dashboard.html`) that gives Nike's CMO everything they need to understand the brand and its competition at a glance.

**This handoff ships as a 4-file package — read all of them before writing code:**

| File | Role |
|---|---|
| `NIKE-DASHBOARD-HANDOFF.md` | This file — goal, IA, constraints, definition of done |
| `nike-dashboard-data.json` | **The machine-readable dataset.** Load/embed this verbatim as `const DATA`. Do NOT transcribe numbers from prose — every value, schema, caveat, and locked-item is already structured here (`_meta._schemas` documents the array layouts). |
| `NIKE-CHART-SPECS.md` | **Per-chart build spec.** Every chart: form, JSON data path, encoding, axes, validated colors, mark specs, annotations, tooltips, layout grid positions, and guardrails. If a visual isn't specified there, don't invent it. |
| `NIKE-CMO-MASTER-REPORT.md` | Human-readable narrative + provenance. Use for context, the red-flag rationale, and Data Notes content; the JSON supersedes it for numbers. |

Do not invent, extrapolate, or "refresh" any figure. Snapshot date: 21 July 2026.

---

## 1. Tech constraints

- **One HTML file.** Inline all CSS and JS. No build step, no localStorage/sessionStorage (not supported in the render environment — use in-memory JS objects only).
- Charts: hand-rolled SVG preferred (full control over the mark specs in `NIKE-CHART-SPECS.md`); Chart.js from cdnjs acceptable if you enforce those specs (thin bars, rounded data-ends, 2px gaps, direct labels, single axis).
- Embed `nike-dashboard-data.json` verbatim as `const DATA = {...}` at the top of the script. It already carries numeric values for plotting plus display strings (`d` fields) for rendering — use `d` when present, otherwise format with a shared `fmt()` helper (K/M abbreviations matching source style).
- Must render fully offline after first load; degrade gracefully if the CDN fails (numbers/tables still visible without charts).
- Responsive: usable at 1440px (primary, boardroom screen) and 390px (phone). Print-friendly is a plus.

## 2. Information architecture

Sticky top nav with 6 tabs (single-page, JS tab switching):

1. **Executive Overview** (default)
2. **Social Competitive**
3. **Search & Keywords**
4. **AI Visibility**
5. **Backlinks & Risk**
6. **Data Notes**

Global header: "NIKE — Brand & Competitive Intelligence" · snapshot badge "Data: 20–21 Jul 2026" · sources badge "IMAI + SEMrush".

### Tab 1 — Executive Overview
The 30-second read. Contents:
- **KPI row (6 stat tiles):** Total social followers 297.6M · Organic traffic WW 120.7M (-0.1%) · Site visits 91.5M (-1.82%) · AI Visibility 85/100 · Authority Score 99 · Backlink toxicity HIGH (⚠ styled as alert tile).
- **"Red flags" alert panel** — render the 6 CMO red flags from master report §1.2 as an ordered, scannable list with severity dots (red/amber).
- **Competitive position strip:** horizontal bar chart of IG followers (log scale or broken axis — Nike 291.7M dwarfs rivals; note the scale choice visibly) and a second bar of engagement rate where Adidas (0.28%) beats Nike (0.06%).
- **Mini trend sparklines:** organic keywords -5.6%, avg likes +22.21%, paid keywords +27%.

### Tab 2 — Social Competitive
From master report Part 2.
- **Scoreboard table** (§2.1) — all 6 brands × all rows. Highlight best-in-row (green) and worst-in-row (red). Nike column visually anchored (bold/left).
- **Charts:**
  a. Grouped bars: IG engagement rate by brand.
  b. Grouped bars: follower growth %/mo by brand (NB +1.43% leader, Nike negative).
  c. Scatter: credibility % (x) vs Audience Quality Score (y), bubble = IG followers — shows Nike as huge-but-low-quality outlier.
  d. Stacked/diverging bars: audience gender split per brand (Lululemon 88% F vs UA 77% M).
  e. Bars: EMV total by brand (Nike $4.8M dominates).
- **Brand cards** (6 expandable cards, one per brand, from §2.2): handle, HQ, platform counts, key campaign faces, hashtags, top post + likes, one-line positioning. Include each brand's currency caveat where relevant.

### Tab 3 — Search & Keywords
From Part 3.
- **KPI row:** WW vs US organic traffic, keywords (with negative deltas in red), US traffic cost $16.0M, traffic share 79%, paid traffic +8.2%.
- **Top keywords table** (§3.2) sortable, with KD% rendered as small meter.
- **Organic competitors chart** (§3.3): horizontal bars of competition level — annotate the insight that retailers (Foot Locker 46%) outrank brand rivals (Adidas 9%).
- **Keyword Gap panel** (§3.4): segment breakdown (Shared/Missing/Weak/Strong/Untapped/Unique) as a stacked bar, "Top missing opportunities" list with volumes, and head-to-head sample table vs Foot Locker.
- **Web engagement row:** visits, UV, pages/visit, duration, bounce (Jun 2026 + deltas). 6-mo visits trend note (~70M→56M→58M) as a simple line with the three known anchor points — label it "approx., chart-read".
- **Position tracking + site health strip:** visibility 82.31%, Top3/10/20/100 donuts; site health 85% gauge with 12 errors / 96 warnings (note: 100-page sample).

### Tab 4 — AI Visibility
From Part 5. This is the novel section — give it room.
- **KPI row:** AI Visibility 85 WW / 91 US ("Great") · Mentions 1.2M (-1.3%) · Citations 379K · Cited pages 485K · Topics 317.1K.
- **Donut:** mentions share by LLM (Gemini 33.5%, ChatGPT 31.3%, AI Mode 24.2%, AI Overview 11%).
- **Bar:** cited pages by engine.
- **Alert callout:** "reddit.com (83.1K) and youtube.com (75.5K) are cited nearly as often as nike.com (83.5K) in AI answers" — this is the CMO takeaway; style prominently.
- **Country table** + opportunity counters (5.9K topic opps, 12.5K source opps).

### Tab 5 — Backlinks & Risk
From Part 4.
- **KPI row:** 35.1M backlinks (-5%) · 199K ref domains (-2%) · AS 99 · Toxicity HIGH.
- **Toxicity panel (alert-styled):** toxic/pot.-toxic/non-toxic split as 100% stacked bar (19.3/15.4/65.2) + list of the 7 worst spam domains; note it's a ~73.5K-link sample.
- **Composition charts:** follow vs nofollow; text vs image; ref-domains-by-AS distribution (heavily skewed to 0-10 = 69% — annotate); TLD split; top countries.
- **Backlink competitors** mini-table (adidas 22%, nba 17%, starbucks 16%, walmart 16%, nfl 15%).

### Tab 6 — Data Notes
Render master report Part 6 verbatim-ish: sources, snapshot date, locked datasets (Advertising Toolkit, Traffic Pro), truncation notes (page-1 samples vs exact totals), the two-backlink-universes explanation, and the "different providers — don't cross-compute" rule. Small type is fine; completeness matters. This tab is what makes the dashboard trustworthy.

## 3. Design system

**Superseded by `NIKE-CHART-SPECS.md` §1–2**, which carries the full token set: surfaces/ink for light + dark, the **machine-validated 6-brand categorical palette** (Nike `#2a78d6`, Adidas `#eb6834`, Puma `#1baf7a`, UA `#eda100`, NB `#e87ba4`, Lulu `#008300` — colorblind-safe, validated both modes, with the mandatory direct-label relief rule for the three sub-3:1 light slots), the reserved status scale, the volt-as-UI-accent-only rule, mark specs, and component anatomies (stat tile, alert tile, meter, table, brand chip, locked cell, source tag, so-what annotation).

The earlier hand-picked brand colors in previous drafts (`#2A6FB0`, `#C6373C`, `#6B7280`…) are **retired** — `#6B7280` fails the chroma floor (reads as gray) and the set was never CVD-validated. Use only the palette in the chart specs; if you change any hex, re-validate before shipping.

## 4. Data integrity rules (hard requirements)

1. Numbers come only from `NIKE-CMO-MASTER-REPORT.md`. If a value isn't there, show a locked/— state.
2. Keep provider boundaries: IMAI (social) and SEMrush (search/web/AI/links) numbers never combine into computed ratios.
3. Follower-scale charts must handle Nike's 291.7M vs rivals' 5.8–29.6M honestly (log scale, broken axis, or "×" multiple labels — say which on the chart).
4. Truncation transparency: tables sampled at page 1 must show "showing top N of {exact total}".
5. Currency labels: adidas £, lululemon CAD$, others $; EMV always USD.
6. The Jun-2026 traffic trend has only 3 chart-read anchor points — do not draw fake monthly precision.

## 5. Definition of done

- Opens as a single file, all 6 tabs functional, no console errors, charts render (and numbers survive CDN failure).
- A CMO can answer these in <60 seconds from Tab 1: Are we growing? (search: flat/declining, social: quality problem) · Who's beating us and where? (Adidas on ER, NB on growth, retailers on SERPs) · What's on fire? (toxic backlinks, fake followers, AI-answer dependence on Reddit/YouTube) · What's the AI-search picture?
- Every red flag from §1.2 is findable within 2 clicks.
- Data Notes tab fully populated.

## 6. Suggested build order

1. Skeleton + tab shell + design tokens from `NIKE-CHART-SPECS.md` §1.
2. Paste `nike-dashboard-data.json` as `const DATA` (no transcription — it's ready).
3. Build the shared components (§2 of chart specs): stat tile, alert tile, meter, table, brand chip, tooltip layer.
4. Executive Overview (E1–E6), then Social (S0–S7), Search (K0–K8), AI (A0–A4), Backlinks (B1–B8), Notes — each chart exactly per its spec ID.
5. Dark mode pass (stepped dark palette, not an invert).
6. Run the chart-spec §5 guardrail checklist, responsive pass (1440/390), CDN-failure fallback, final QA of ~20 spot-checked values against `DATA`.
