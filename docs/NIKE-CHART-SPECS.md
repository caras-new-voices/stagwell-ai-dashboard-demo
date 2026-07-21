# NIKE CMO DASHBOARD — CHART & COMPONENT SPECIFICATIONS

Companion to `NIKE-DASHBOARD-HANDOFF.md`. Every chart on the dashboard is specified here: form, data binding (JSON paths into `nike-dashboard-data.json`), encoding, colors, marks, annotations, and interactions. If a visual isn't specified here, don't invent it — ask or omit.

Method note: forms were chosen job-first (magnitude → bar, identity → categorical, polarity → diverging, part-to-whole → stacked bar, single value → stat tile), and the palette below was machine-validated for colorblind safety and contrast in both light and dark modes. Do not substitute colors without re-validating.

---

## 1. DESIGN TOKENS

### 1.1 Surfaces & ink (CSS custom properties)
```css
.dash {  /* light (default) */
  --surface-1:#fcfcfb; --page:#f9f9f7;
  --ink-1:#0b0b0b; --ink-2:#52514e; --ink-muted:#898781;
  --grid:#e1e0d9; --baseline:#c3c2b7; --border:rgba(11,11,11,.10);
  --delta-up:#006300; --delta-down:#d03b3b;
  --header-band:#111111; --accent-volt:#CFF000; /* UI accent ONLY — see 1.4 */
}
.dash[data-theme="dark"] {
  --surface-1:#1a1a19; --page:#0d0d0d;
  --ink-1:#ffffff; --ink-2:#c3c2b7; --ink-muted:#898781;
  --grid:#2c2c2a; --baseline:#383835; --border:rgba(255,255,255,.10);
  --delta-up:#0ca30c; --delta-down:#d03b3b;
}
```
Typography: `system-ui, -apple-system, "Segoe UI", Helvetica, sans-serif` everywhere. Section headers uppercase, letter-spacing 0.04em. Stat values ≥40px; hero figures ≥48px. `font-variant-numeric: tabular-nums` on table columns and axis ticks only.

### 1.2 Brand series palette (categorical — VALIDATED)
Fixed brand→slot mapping. Never reassign, never cycle, never repaint when a filter removes brands.

| Brand | Light | Dark | Slot |
|---|---|---|---|
| Nike | `#2a78d6` | `#3987e5` | 1 (blue) |
| Adidas | `#eb6834` | `#d95926` | 2 (orange) |
| Puma | `#1baf7a` | `#199e70` | 3 (aqua) |
| Under Armour | `#eda100` | `#c98500` | 4 (yellow) |
| New Balance | `#e87ba4` | `#d55181` | 5 (magenta) |
| Lululemon | `#008300` | `#008300` | 6 (green) |

**Validation receipts** (validate_palette.js, 21 Jul 2026): light — ALL PASS, worst adjacent CVD ΔE 9.1, normal-vision 19.6; dark — ALL PASS, worst CVD 8.4, all ≥3:1 contrast. Light-mode WARN: Puma `#1baf7a` (2.74:1), UA `#eda100` (2.11:1), NB `#e87ba4` (2.62:1) sit below 3:1 on the light surface → **relief rule: every chart using these fills must carry visible direct labels, and the scoreboard table is always one click away.** This is mandatory, not optional.

Rationale for non-brand-literal colors: the athletic set's actual identities are all black/red/white — brand-literal hues cannot do identity work. A persistent legend + brand chips (logo initial in slot color) carry the mapping; keep the legend order identical to `social.brandOrder`.

### 1.3 Emphasis mode (Nike-vs-field charts)
When the story is "Nike vs everyone" rather than "compare six brands": Nike in slot-1 blue, all competitors in `#c3c2b7` (light) / `#52514e` (dark) de-emphasis gray, competitor bars direct-labeled in ink. Use emphasis for: E3 (share strip), A3 (cited sources). Use full categorical for: S1–S6 comparisons.

### 1.4 Reserved colors
- **Volt `#CFF000` is a UI accent only** — nav active state, section rules, header band highlights. NEVER a data series (fails contrast on white; would impersonate a status).
- **Status scale (fixed, icon+label always):** good `#0ca30c` · warning `#fab219` · serious `#ec835a` · critical `#d03b3b`. Used ONLY where data means good/bad: toxicity split (B2), red-flag severity dots (E2), delta coloring. Never for "series 4".
- Sequential ramp (single-hue magnitude, e.g. KD meters, AS distribution): blue steps `#cde2fb → #0d366b` (light anchor ≥ step 250 `#86b6ef` for ordinal bars).

### 1.5 Mark specs (all charts)
Bars: thin (≤32px), 4px rounded corners on the data end only, flat at baseline; 2px surface-color gap between adjacent bars and between stack segments. Lines: 2px, no smoothing beyond monotone. Points: ≥8px. Gridlines: hairline `--grid`, horizontal only, behind marks. Axis text 11–12px `--ink-muted`. Direct labels: 12px `--ink-2` (never in series color), placed at bar end. One y-axis per chart — never dual-axis; two measures = two charts.

---

## 2. COMPONENT SPECS

**Stat tile** — `[LABEL 11px uppercase muted] / [VALUE 40px ink-1] / [delta chip] / [optional 60×24 sparkline]`. Delta chip: `▲ +8.2%` in `--delta-up` / `▼ -5.6%` in `--delta-down`, 12px, chip background at 8% tint. White card, 12px radius, hairline border, 20px padding.

**Alert tile** — stat tile variant for risk values: 3px left border in status color, status icon + label (never color alone). Used for toxicity HIGH, AQS 14.

**Meter** — horizontal track (`--grid`), fill in sequential blue (or status color when the value means good/bad, e.g. site health 85% → good green), value labeled at right in ink. Height 8px, 4px radius. Never a 2-slice pie.

**Table** — header row 11px uppercase muted with hairline bottom border; rows 13px, 8px vertical padding, hairline separators; numeric columns right-aligned tabular-nums; best-in-row cell gets a `▲` tint (good green at 10%), worst-in-row `▼` (critical at 8%) — icon + tint, not color alone. Sortable columns get ↕ affordance.

**Brand chip** — 16px round swatch in the brand's slot color + brand name in ink. Appears in every legend, table row, and card header. This is the identity carrier.

**Locked cell** — `—` + 🔒 with `title` tooltip naming the unlock (from `locked[]`). Never render a fake value.

**Source tag** — 10px muted, bottom-right of every card: `IMAI · 21 Jul 2026` or `SEMrush · 21 Jul 2026`.

**"So what" annotation** — one 13px ink-2 line under every chart title stating the insight (provided per chart below), not a description of the encoding.

**Tooltips (all charts)** — per-mark hover: brand chip + metric name + display value + delta if present. Hit target ≥ the mark, min 24px. Line charts: crosshair + shared tooltip. Charts must render values without hover too (direct labels / table) — hover is additive.

---

## 3. LAYOUT GRIDS (desktop 1440px; 12-col, 24px gutter; stack to 1-col at <768px)

**Tab 1 Executive:** row1 = 6 stat tiles (2 cols each). row2 = E2 red flags (span 5) + E3 share strip (span 7). row3 = E4 ER bars (span 4) + E5 growth bars (span 4) + E6 sparkline trio (span 4).
**Tab 2 Social:** row1 = S0 scoreboard table (span 12). row2 = S1 (6) + S2 (6). row3 = S3 scatter (7) + S4 gender (5). row4 = S5 EMV (6) + S6 quality (6). row5 = 6 brand cards (span 4 each, 2 rows).
**Tab 3 Search:** row1 = 5 stat tiles. row2 = K1 keywords table (span 7) + K2 competitors (span 5). row3 = K3 gap stacked (7) + K4 opportunities (5). row4 = K5 web engagement tiles (6) + K6 visits line (6). row5 = K7 rank buckets (6) + K8 site health (6).
**Tab 4 AI:** row1 = 5 stat tiles. row2 = A1 LLM share (7) + A2 cited pages (5). row3 = A3 cited-sources emphasis (7) + A4 country table (5).
**Tab 5 Backlinks:** row1 = 4 stat tiles. row2 = B2 toxicity (7) + B3 worst offenders list (5). row3 = B4 AS distribution (7) + B5 attributes duo (5). row4 = B6 TLD (4) + B7 countries (4) + B8 similar profiles (4).
**Tab 6 Notes:** prose, two columns.

---

## 4. CHART SPECS

### Tab 1 — Executive Overview

**E1 · KPI row** — 6 stat tiles.
Data: `social.brands.nike.totalFollowers` · `search.domainOverview.worldwide.organicTraffic` · `search.trafficAnalytics.visits` · `ai.visibility.worldwide` (render "85 / 100 · Great") · `search.domainOverview.worldwide.authorityScore` · `backlinks.audit.overallToxicity` (alert tile, critical).

**E2 · Red-flag panel** — not a chart; ordered list from `redFlags[]`. Severity dot in status color + icon (critical ⛔, serious ⚠, warning ▲) + title bold + detail. Each row links to its `tab`.

**E3 · Share of six-brand IG audience** — single horizontal 100% stacked bar, part-to-whole.
Data: `platforms.instagram.followers` per brand → shares: Nike 81.6%, Adidas 8.3%, Puma 3.6%, NB 2.6%, UA 2.3%, Lulu 1.6% (computed from JSON, label with both % and absolute). Brand slot colors, 2px gaps, segments <5% labeled via leader lines or a side list. So-what: "Nike is 82% of the six-brand Instagram audience — scale is not the problem."
**Do NOT draw a per-brand linear bar chart of follower counts** — a 291.7M bar makes the rest unreadable; the share strip + the scoreboard table carry this honestly.

**E4 · IG engagement rate by brand** — horizontal bars, categorical (brand colors), sorted desc.
Data: `postData.engagementRatePct` per brand. X-axis 0–0.30%, 0.05 ticks. Direct-label every bar (relief rule). So-what: "Adidas converts attention 4.7× better than Nike."

**E5 · Follower growth %/mo** — horizontal bars from a zero baseline, brand colors; negative bars extend left of the zero line (Nike -0.07 is the only negative — annotate it).
Data: `influencerInsight.growthPctMo` per brand. So-what: "New Balance is compounding fastest; Nike is the only shrinking account."

**E6 · Momentum sparklines** — 3 micro stat-tiles: Organic keywords `-5.6%` (down/red), Avg IG likes `+22.21%` (up/green), Paid keywords `+27%` (up/green). Sparkline optional; the signed delta is the content.

### Tab 2 — Social Competitive

**S0 · Scoreboard table** — the full §2.1 matrix; every metric row from the master report, brands as columns, Nike column first with 2px slot-1 left border. Best/worst-in-row treatment per component spec. This table is also the relief channel for all sub-3:1 fills. Data: assemble from `social.brands.*`.

**S1 · Engagement per post by platform** — grouped horizontal bars per brand (groups = brands, bars = IG/TikTok/YouTube).
Data: `engagementPerPost`. Platform encoding: IG solid brand color, TikTok 60% tint, YouTube 30% tint (one hue per brand, shade = platform — keeps identity with brand, ordinal shade for platform). Direct-label TikTok bars for Puma (90.6K) and Lulu (17.7K). So-what: "Puma and Lululemon earn most engagement on TikTok, not Instagram."

**S2 · Follower credibility & bots** — paired horizontal bars per brand: credibility % (brand color) and fake/bot % (critical `#d03b3b` at 70%, with ⚠ icon — status meaning).
Data: `influencerInsight.credibilityPct`, `followersBreakdown` row "Fake/bots". Sort by bots desc → Nike lands on top. So-what: "1 in 5 Nike followers is fake — double the set average."

**S3 · Quality map** — scatter, x = credibility % (68–80), y = Audience Quality Score (0–50), bubble r ∝ √(IG followers), min r 8px.
**Single-series treatment:** all bubbles `--ink-2` gray at 60% with a brand chip label at each point; Nike's bubble in slot-1 blue (emphasis). Identity via direct labels, NOT via 6 colors (all-pairs scatter caps at 3 validated colors — labels are the safe channel). Quadrant guides at x=74, y=30, corner captions ("big but hollow" bottom-left …). So-what: "Nike is the giant outlier in the low-quality quadrant."

**S4 · Audience gender split** — per-brand 100% stacked horizontal bars, 2 segments: Female slot-5 magenta, Male slot-1-blue-300 tint `#6da7ec`; 2px gap; both segments direct-labeled with %.
Data: `audience.gender`. Order by female share desc (Lulu 88% top, UA 23% bottom). So-what: "Lululemon owns the female audience the rest of the set can't reach."

**S5 · Earned media value** — horizontal bars, brand colors, log-free: linear scale is fine ($102.9K–$4.8M spans ~47×; use $0–$5M axis, direct-label all).
Data: `emvUSD.total`. Optional stacked composition (post/reels/story) as 3 shades of the brand hue. So-what: "Nike's content earns 9× the media value of the whole rest of the set combined."

**S6 · Audience Quality Score** — horizontal bars 0–100 axis, colored by STATUS band not brand (0–25 critical, 26–50 warning, 51–75 good-tint, 76+ good) with the numeric score + label direct-labeled, brand chip on the left.
Data: `imaiScore.audienceQuality`. So-what: "Nobody in the set exceeds 'Fair'; Nike is last at 14."

**S7 · Brand cards** (6) — header: brand chip + handle + country + currency badge. Body rows: platforms (icons + counts), growth + credibility + AQS mini-stats, positioning line, hashtags as chips (max 6), faces (`mentions` top 4), top post (caption truncated 80ch + likes), audience one-liner (gender/top country/top age band from `audience`). Footer: IMAI source tag. Expandable (`<details>`) full audience tables: ageGender grouped bars (F slot-5 / M blue-300, per S4 encoding), cities/countries/ethnicity/language/reachability/affinity as label+meter rows.

### Tab 3 — Search & Keywords

**K0 · KPI row** — WW organic traffic 120.7M (-0.1%) · US organic traffic 35.7M (+0.31%) · US keywords 4.2M (-5.04%, red) · US traffic value $16.0M · Traffic share 79%. Data: `search.domainOverview`.

**K1 · Top organic keywords table** — columns Keyword / Pos / Traffic / Traffic % / Volume / KD. KD renders as 40px sequential-blue meter + number. Position 1 gets a subtle good-tint chip. Sortable (default traffic desc). Footer: "Showing 42 of 4,180,334 — page 1 of 41,804 (SEMrush UI cap)". Data: `search.topKeywordsUS.rows`.

**K2 · Organic competitors** — horizontal bars of competition level %, top 12 + the 4 brand rivals appended; single-hue sequential blue (these entities don't recur elsewhere — no categorical spend), except adidas/UA/NB/Lulu bars get their brand slot colors + chip to tie back to the social set. Retailer bars get a subtle "RETAIL" tag. Data: `search.organicCompetitorsUS.rows`. So-what: "Nike's SERP war is against its own retail channel — Foot Locker 46% overlap vs Adidas 9%."

**K3 · Keyword gap composition** — horizontal 100% stacked bar of `keywordGapVsFootlocker.nikeSegments` (Shared/Missing/Weak/Strong/Untapped/Unique). Ordinal single-hue blue ramp light→dark in listed order, direct-label each segment ≥5%. Under it, the head-to-head sample table (`headToHeadSample.rows`, columns Keyword / Nike pos / FL pos / Volume / KD / CPC) with loser-position cells tinted warning. Footer: "sample page 1 of 2,479".

**K4 · Missing-keyword opportunities** — horizontal bars, volume, single-hue blue; each labeled with volume ("new balance — 1.83M"). Data: `keywordGapVsFootlocker.topOpportunities`. So-what: "Five rival-brand terms Nike doesn't rank for total 6.5M monthly searches."

**K5 · Web engagement tiles** — 5 stat tiles from `search.trafficAnalytics` (visits, UV, pages/visit, duration, bounce — bounce delta -2.14% is GOOD, color it green and note "lower is better").

**K6 · Visits trend** — line, 3 anchor points from `sixMonthTrendAnchors.points`, drawn DASHED with visible point markers and the caption "approximate — chart-read anchors only". Y-axis 0–84M. Never render as a smooth 6-point monthly line. So-what: "Traffic dipped ~20% into spring and only partially recovered."

**K7 · Rank buckets** — 4 meters (Top 3/10/20/100 → 83/87/87/87 of tracked set) + visibility hero "82.31%". Data: `search.positionTracking`.

**K8 · Site health** — health 85% meter (good green) + errors 12 (critical chip) + warnings 96 (warning chip) + crawled-pages 100% stacked bar (`pageBreakdown`: healthy good / broken critical / have-issues warning / redirect blue-300 / blocked gray). Caption: "100-page sample crawl". Data: `search.siteAudit`.

### Tab 4 — AI Visibility

**A0 · KPI row** — AI Visibility "85 WW / 91 US · Great" · Mentions 1.2M (-1.3%) · Citations 379K (+0.3%) · Cited pages 485K (+2.1%) · Topics 317.1K. Data: `ai.*`.

**A1 · Mentions share by LLM** — horizontal 100% stacked bar (part-to-whole, NOT a donut), 4 segments in slot order 1–4 (Gemini blue, ChatGPT orange, AI Mode aqua, AI Overview yellow), all direct-labeled "Gemini 33.5%". 2px gaps. Data: `ai.byLLM.rows`. So-what: "Gemini and ChatGPT together drive two-thirds of Nike's AI presence."

**A2 · Cited pages by engine** — horizontal bars, same engine→slot colors as A1 (color follows entity across the tab). Values 385.2K / 83.2K / 31.2K / 27.9K. So-what: "ChatGPT cites Nike pages 4.6× more than all other engines combined — optimize for its crawler first."

**A3 · Who AI cites for 'Nike'** — 3 horizontal bars, EMPHASIS mode: nike.com slot-1 blue, reddit.com and youtube.com de-emphasis gray, all direct-labeled (83.5K / 83.1K / 75.5K). Render inside a warning-bordered callout card. Data: `ai.topCitedSources`. So-what: "Nike does not control its own AI narrative — Reddit is 400 mentions behind."

**A4 · Geography + opportunities** — table from `ai.byCountry` (+ `countryVisibility` column) and two stat tiles: Topic opportunities 5.9K, Source opportunities 12.5K.

### Tab 5 — Backlinks & Risk

**B1 · KPI row** — Backlinks 35.1M (-5%) · Ref domains 199K (-2%) · Authority 99 · Toxicity HIGH (alert tile). Data: `backlinks.headline`.

**B2 · Toxicity split** — single 100% stacked horizontal bar in STATUS colors: toxic critical / potentially-toxic warning / non-toxic good, icon+label per segment, direct-labeled with % and counts. Caption: `audit.sampleNote`. Data: `backlinks.audit.toxicitySplit`. So-what: "One in three sampled links is toxic or suspect."

**B3 · Worst offenders** — list of `audit.worstOffenders` domains, monospace, each with critical dot + "Toxicity 100". Plus the new/broken/lost movement mini-table (`audit.refDomains`, `audit.analyzedBacklinks`).

**B4 · Ref domains by Authority Score** — vertical bars, ordinal: buckets 0-10 → 91-100 in order, single-hue blue ramp light→dark (low AS light, high AS dark). Log-free; 69% bar dominates — direct-label all ten with % and count. Data: `backlinks.refDomainsByAS.rows`. So-what: "69% of linking domains have almost no authority — volume without weight."

**B5 · Composition duo** — two 100% stacked bars: link attributes (follow 86 / nofollow 14, blue 500/blue 250) and backlink types (text 65 / image 34 / other, blue ramp). Direct-labeled. Data: `backlinks.attributes`, `backlinks.types`.

**B6 · TLD split** — label+meter rows (.com 65% …), sequential blue. Data: `backlinks.tld`.

**B7 · Ref-domain countries** — label+meter rows, US 66% first. Data: `backlinks.topCountries`.

**B8 · Similar backlink profiles** — mini-table domain + competition meter (adidas 22% …). Data: `backlinks.similarProfiles`. Adidas row gets its brand chip.

### Tab 6 — Data Notes
Render `_meta`, `locked[]` (as a table: item / unlock / cost), the truncation notes embedded in each dataset's `note` fields, the provider rule, and the master report's Part 6 caveats. Include the palette-validation receipt line from §1.2.

---

## 5. GUARDRAILS (checked before "done")

1. One y-axis per chart, everywhere. Two measures → two charts.
2. No linear per-brand follower bar (E3 note). No donut where a stacked bar is specified. No pie with 2 slices.
3. Brand colors never reassigned or cycled; a filter that hides brands must not repaint the rest.
4. Sub-3:1 fills (Puma/UA/NB light slots) always ship with direct labels — verify by removing hover and checking every value is still readable.
5. Status colors appear only in B2, S6 bands, S2 bots bar, alert tiles, deltas, severity dots — never as a 7th series.
6. Text never set in series color; labels are ink on surface.
7. Locked data renders locked (🔒 + tooltip), truncated tables state "showing N of TOTAL", and the 3-anchor traffic line stays dashed-approximate.
8. Dark mode is the stepped dark palette above, not a CSS invert; re-check the three relief slots still hold (they pass ≥3:1 in dark — labels remain anyway).
9. Every card has a source tag; every chart has its so-what line.
10. Final pass: screenshot at 1440px and 390px, check label collisions, legend order = `social.brandOrder`, and tab-switch preserves scroll position.
