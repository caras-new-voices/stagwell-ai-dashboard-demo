# CHANGES — Stagwell AI · Nike Intelligence Demo (design polish pass)

Applied to `index.html` (single self-contained file preserved). No data, copy meaning,
chart types, or behavioral contracts changed. Brand-series palette untouched (no hex
changed → no re-validation needed).

## Iconography (debt #1 — highest leverage)
- Added an inline-SVG icon set (`IC` map + `ic(name,size)` helper) — stroke-based, 1.9px,
  currentColor. Replaced **every emoji used as UI iconography**: nav links, hero suggestion
  cards, Nike-only notice, onboarding step pills, snapshot card headers, CMO red-flag
  severity markers, priorities picker, KPI alert marker, statchips (errors/warnings/
  cannibalization), toxicity + AI callout headers, all 14 Stagwell Solutions product marks
  + 4 category headers, chat header/close/send, chat FAB, generation overlay logo.
- Emoji locks 🔒 → SVG lock glyph via `--lockicon` mask; ✦ solve-pill / so-what / chart-note
  bullets → SVG spark mask.
- Kept ▲ ▼ • as **data-direction glyphs** (deltas, best/worst, scoreboard) — geometric
  shapes that render identically cross-OS, not emoji.

## Brand identity (debt #2)
- Gold spark mark added to the wordmark, generation overlay, and chat.
- Added `<link rel=icon>` (data-URI gold-square spark), `<meta name=theme-color>`,
  description + OG tags.

## Typography (§4.1)
- Refined system stack (adds Segoe UI Variable), `text-rendering:optimizeLegibility`,
  tabular numerals on all numeric elements, tighter display tracking, KPI value scale
  normalized (36/26px), section headers recased.
- **No embedded WOFF2 was added.** The single-file / no-network rule means a font would
  have to be base64-inlined, and I have no licensed subsettable WOFF2 to embed offline
  without a build/subset step. Flagging as an open option: drop a subsetted latin WOFF2
  (one weight, ≤40KB) into a `@font-face` if the team wants a distinct display face.

## Component polish
- **Chat (debt #3):** suggestion chips anchored directly under the greeting; subtle spark
  watermark fills the remaining space (no dead gap); animated 3-dot typing indicator;
  "Scripted demo" rendered as a proper badge; message bubbles get soft elevation.
- **Footer CTA (debt #4):** now slimmer and **per-tab** — each tab gets contextual copy +
  button label instead of the identical dark band.
- **Scoreboard (debt #5):** row grouping (Reach / Engagement / Audience quality /
  Economics & audience), calmer best/worst tints, Nike column tint + border, sticky first
  column on mobile.
- **Solutions (debt #7):** mini-cards (QuestRQ/Unlock/ID Graph) moved to their own tighter
  grid so they no longer orphan single-card rows; contact strings wrap safely
  (`overflow-wrap:anywhere`).
- **KPI tiles (debt #12):** value sizing normalized; delta chips no longer wrap under the
  sub-label (flex sub row).
- **Skeletons (debt #16):** loading state now mirrors real geometry (bar-row shapes +
  chart block) instead of generic tiles.
- **Mobile (debt #14):** scroll-fade masks on the tab strip and table wrappers; KPI tiles
  go 2-up at ≤640px; chat FAB shrunk/repositioned so it clears CTAs.
- **Dark mode (debt #13):** audited callout/tint pairs; notice + Nike-column tints tuned
  for the dark surface.

## Motion
- No timing changes to `GEN` or CSS custom props; reduced-motion instant path intact
  (added typing-dot animation to the reduced-motion disable list).

## Not yet done (candidates for a follow-up)
- Per-tab bespoke skeletons that mirror each specific layout (current is one improved
  generic shape).
- Scatter-label collision avoidance (debt #10) and dedicated validated F/M gender pair
  (debt #11) — both touch data-adjacent logic; left for a validated round.
- Designed one-pager print stylesheet for the Executive tab (debt #15).

---

## Integration pass (applied on top of the design polish)

**Brand palette restored to the approved Stagwell system.** The design pass had shifted
the chrome to an indigo primary (`#5b4fe6`) with muted ochre gold (`#C9A24B`) and
indigo-tinted neutrals — undisclosed in the notes above and contrary to the approved
brand direction. Restored: gold `#FFB81C`/`#E5A200` (dark `#FFC23D`), deep teal-navy
`#04222E`/`#0a3341`, teal interactive `#0A5E86` (dark `#5fb8d6`), warm neutrals, and the
spec-fixed status scale. All component work (SVG icon set, spark mark, chat/scoreboard/
skeleton/mobile/footer polish, favicon + meta) kept. Data-series palette untouched.

**Company showcase modal (new).** Clicking any ✦ product pill anywhere on the dashboard
now opens an in-page profile modal — no tab switch. Clicking a company card on the
Stagwell Solutions tab opens the same modal with the full one-pager detail:
- problem statement, description, and the FOR-NIKE tie-in
- **Capabilities — from the one-pager**: all four features with their full deck copy
- **Where this shows up on your dashboard**: evidence rows that deep-link to the exact
  tab carrying that metric (closes the modal, switches tab)
- contact, "See in catalog" (jumps + flash-highlights the card), "Request a demo" (chat)
Backdrop / ✕ / Esc close; body scroll locks while open; reduced-motion skips the entrance
animation. Mini products without one-pagers (QuestRQ, Unlock, ID Graph) state that
honestly in the modal.

---

## Brand-accuracy pass (against the real stagwellglobal.com)

Compared against Stagwell's actual site and official tri-bar logo asset:

- **Real Stagwell logo added** — a faithful inline-SVG recreation of the tri-bar "S" mark
  (yellow `#FBB61A` / teal `#0098B9` / ink `#231F20`, correct bar geometry and corner
  radii), rendered on a white app tile in the top nav, the generation overlay, the chat
  header, and the favicon.
- **Teal is now the primary interactive color** (`#0098B9`, link `#0E7C99`, dark
  `#2FA9C8`) — buttons, links, active tab underline, selection states, progress bars,
  section ticks, chat send/FAB — matching the site's teal-accent hierarchy.
- **Navy corrected** to the wordmark ink `#12333F`/`#1A4A5F` (was a too-green `#04222E`);
  neutrals shifted from warm to cool gray; dark theme re-stepped to navy-cool.
- **Yellow recalibrated to the logo's `#FBB61A`** and reserved for brand/solution
  moments: DEMO + data badges, the NIKE title accent, and all Stagwell-solution surfaces
  (✦ pills, evidence rows, FOR-NIKE blocks, expert CTAs) — mirroring how yellow behaves
  in the logo: present, not dominant.
- Search hero kept light (per the design pass — and truer to the white-first site), with
  navy headline + teal accent phrase and a yellow→teal hairline underline on dark bands.
- Data-series palette and status colors untouched.

---

## Feedback round 1 (22 Jul 2026)

Owner-approved redesign from Jonathan Caras' review. Light theme only, dot map built,
all sprints executed. Data (`const DATA`), the CVD-validated series palette, status
colors, chart guardrails, the Nike-only gate, the scripted-chat contract, and the
Solutions modal system were left untouched.

### Sprint A — Arrival page
- New headline **"Enter a new world of brand intelligence."** (navy ink, teal accent on
  the phrase) with a quiet sub-line. Placeholder → "Type in any brand…".
- Deleted the four suggestion cards and the coach-mark bubble (markup + CSS + JS wiring).
- Added a pure-CSS ambient atmosphere: two large blurred teal/gold gradient blobs
  drifting slowly, plus an oversized, very faint tri-bar logo motif offset at the edge.
  All motion disabled under `prefers-reduced-motion`.
- Stripped the "Public Intelligence · New" and "Examples" nav links; kept logo + wordmark
  + DEMO badge (+ user chip once an email is entered).

### Sprint B — Single theme + generation overlay
- Removed the theme toggle button, `state.theme`, the toggle JS, and every
  `html[data-theme="dark"]` CSS block (17 rules + the dark token set).
- Generation overlay keeps the step checklist; removed the bottom progress bar
  (`.gen-bar` markup + its JS width updates). Added a CSS-only rotating conic gradient
  ring (teal→gold) behind the pulsing logo tile as the "alive" signal.
- Final step copy → **"Assembling your dashboard"**.

### Sprint C — Land straight on a starter dashboard
- New flow: search → generation overlay → dashboard. The snapshot / priorities / signup
  `<section>`s and their renderers were deleted from the flow.
- Dashboard opens in a **starter state**: Overview tab only + a CTA pseudo-tab
  **"⊕ Build out the rest of your dashboard"**; no chat panel, no FAB.
- Overview recomposed top-down: KPI hero row → **"Where Nike is winning" / "What needs
  attention"** twin panels (green/red headers, icon + stat + one-line why; every value
  verbatim from `DATA`) → **the dot map** → existing share-of-6-brand strip, ER chart,
  follower-growth chart, red-flags panel, momentum sparklines (all intact).
- **Self-contained SVG dot-grid world map** (`720×300` viewBox, ~1134 dots generated at
  build time from hand-defined coarse continent polygons rasterized at ~3.5°, inlined as
  a flat integer array; Antarctica omitted; no external data). Four provider-pure layers
  switched by chips — **Audience/IMAI**, **Likers/IMAI**, **AI mentions/SEMrush**,
  **Backlink origins/SEMrush** — each with teal centroid highlights + haloes, direct
  labels with exact values, a "so what" line and a source tag. Providers never mixed.

### Sprint D — Staged in-place expansion
- The build-out CTA opens an in-place modal: **Beat 1** priorities picker (the existing 7
  priority cards, same `state.priorities` logic) → **Beat 2** single optional email field
  (initials → user chip) → **Beat 3** short "Generating your full dashboard" overlay, then
  the six remaining tabs animate into the tab bar one-by-one (~120ms stagger) and the
  priorities chips appear in the header → **Beat 4** the **"Consult Stagwell AI"** FAB
  appears bottom-right with an attention pulse; chat opens only on click.
- Expansion is one-way per session. Pre-expansion, any deep-link to a not-yet-built tab
  (`.goto`, twin-panel rows, red-flag links, solve-modal navigation) triggers the
  expansion flow instead of dead-ending. Under reduced-motion the whole flow is instant
  (tabs appear at once, no pulses).

### Sprint E — Cleanup, QA, ship
- Deleted dead code: snapshot/priorities/signup sections + `renderSnapshot`, the signup
  handler, theme JS/CSS, `.gen-bar`, suggestion-card + coach markup/CSS, removed nav
  links, and `GEN.provision` (replaced by `GEN.buildout`). All timings stay in the `GEN`
  config and CSS custom properties.
- Playwright QA (headless Chromium) at 1440×900, 390×844, and a `reducedMotion:'reduce'`
  context: full flow search→gen→starter→expand (4 beats)→every tab→company modal (via
  solve pill and Solutions card)→chat. All assertions pass; zero console errors and zero
  page errors in every context.
