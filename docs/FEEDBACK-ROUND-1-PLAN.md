# FEEDBACK ROUND 1 — EXECUTION BRIEF (approved)

Source: Jonathan Caras' review transcript, 22 Jul 2026. Decisions locked by the owner:
**light theme only** · **build the dot map** · execute all sprints below.

**You are building on** `index.html` at repo root (single self-contained file: `<style>` →
HTML → `<script>` with `const DATA` then app JS). Read `docs/DESIGN-HANDOFF.md` §2–3 first —
its hard constraints and QA loop still bind: single file, no CDN/network, frozen
CVD-validated data palette, data integrity apparatus untouched, `prefers-reduced-motion`
instant paths, zero console errors, 1440px + 390px.

---

## Sprint A — Arrival page (search screen)

1. Replace the headline with: **"Enter a new world of brand intelligence."** (navy ink,
   big; keep the teal accent treatment on part of the phrase). Sub-line small and quiet:
   "Type in any brand to generate a live intelligence dashboard."
2. **Delete** the four suggestion cards and the coach-mark bubble entirely (markup + CSS +
   their JS wiring).
3. Search field stays center-stage, placeholder "Type in any brand…". The **Nike-only
   notice stays** but only appears on non-Nike submissions (unchanged gate logic).
4. Ambient atmosphere instead of furniture: a slow, subtle background motion on the hero —
   e.g. two large blurred gradient blobs (teal/gold at low opacity) drifting very slowly,
   and/or a faint oversized tri-bar logo motif offset at the edge. Tasteful; the page
   should feel *arrived at*, Google-style. Must be pure CSS, disabled under
   reduced-motion.
5. Top nav: **remove** "Public Intelligence · New" and "Examples" links. Keep logo tile +
   wordmark + DEMO badge (+ user chip when set).

## Sprint B — Single theme + generation overlay

1. **Light theme only.** Remove the theme toggle button, the `theme` state/JS, and every
   `html[data-theme="dark"]` CSS block. The dark-styled surfaces that remain by design
   (top nav, dashboard header band, generation overlay) are unaffected — they're brand
   navy, not "dark mode".
2. Generation overlay: **keep the step checklist** (approved), **delete the bottom
   progress bar** (`.gen-bar` + its JS updates). Replace its "alive" signal with modern
   motion: the pulsing logo tile stays; add a soft conic/gradient ring slowly rotating
   behind the logo tile or a subtle shimmer sweep across the active step row. CSS-only,
   reduced-motion-safe.
3. Re-copy final step: "Assembling your snapshot" → **"Assembling your dashboard"**; sub
   line stays truthful.

## Sprint C — Land straight on a starter dashboard

**New flow: search → generation overlay → dashboard.** The free-snapshot, priorities, and
signup *pages* are removed from the flow (their content is repurposed below; delete the
dead `<section>`s and their nav wiring in Sprint E).

The dashboard opens in a **starter state** ("war room" first view):

1. **No chat**: chat panel closed, no docked auto-open, no FAB yet.
2. **Tab bar shows only** "Overview" plus one CTA-styled pseudo-tab:
   **"⊕ Build out the rest of your dashboard"** (this drives Sprint D). The other six
   tabs do not exist yet.
3. Recompose the Overview (exec) pane top-down as:
   - **KPI hero row** (existing 6 tiles, count-ups kept).
   - **"Where Nike is winning" / "What needs attention" twin panels** (green-tinted /
     red-tinted headers, icon + stat + one-line why, each row deep-linking to its tab
     *after expansion* — before expansion the links are inert/hidden). Winning: IG share
     81.6% of six-brand audience · EMV $4.8M (~9× set) · AI Visibility 85 "Great" ·
     Authority 99 · avg likes +22.21% MoM. Needs attention: backlink toxicity HIGH 19.3% ·
     AQS 14 / 21.65% bots · ER 0.06% vs adidas 0.28% · organic keywords −5.6% ·
     non-branded traffic −2.39% · 1.5M keyword gap vs Dick's. All values verbatim from
     `DATA` (these all exist — do not invent). This panel largely supersedes the red-flags
     list on the starter view; the full red-flags panel (with solve pills) stays in the
     expanded Overview below the fold or merges in — your layout call, no data dropped.
   - **The dot map** (see spec below).
   - Existing share-of-6-brand strip + ER chart + momentum sparklines follow.
4. Keep all existing charts/tabs' content intact for the post-expansion state.

### Dot-map spec (self-contained, no external data)

- Build a **stylized equirectangular dot-grid world map** as inline SVG. Generate the
  landmass lattice at build time yourself: hand-define ~8–12 coarse continent polygons
  (lat/lon vertices; rough is fine — the style is deliberately abstract), rasterize at
  ~2.5–3° spacing with point-in-polygon, and inline the resulting ~700–1200 dots as tiny
  circles (r≈1.6px, `var(--baseline)` at ~55% opacity). One-time script; commit only the
  resulting SVG/coordinate array inline in `index.html`. Antarctica omitted.
- **Data layers** (chips above the map, one active at a time — providers never mixed):
  - **Audience** (IMAI): `DATA.social.brands.nike.audience.countries` — US 19.72,
    BR 7.51, IN 5.50 (+ optionally likers countries IN 30.91/US 11.62/UK 6.68 as a second
    chip "Likers"). Source tag: IMAI · 21 Jul 2026.
  - **AI mentions** (SEMrush): `DATA.ai.byCountry` — US 37.6%/464.7K, DE 4.8/59K,
    ES 4.6/56.9K. Source tag: SEMrush AI SEO.
  - **Backlink origins** (SEMrush): `DATA.backlinks.topCountries` — US 66%, UK 6, FR 4,
    DE 4, JP 3, CA 2. Source tag: SEMrush.
- Highlight = larger teal dots + soft halo at the country's centroid (hardcode a small
  centroid table for US, BR, IN, UK, DE, FR, ES, JP, CA, AU, ID, SE, MX, CN as needed),
  with a small label + exact value. Active layer swaps highlights + so-what line +
  source tag. Direct labels mandatory; hover title tooltips additive.
- Style: page-consistent card, "so what" line under it (e.g. Audience layer: "One in five
  followers is American — but the engaged audience skews to India."). Reduced-motion: no
  halo pulse.

## Sprint D — Staged expansion (in place, one beat at a time)

Clicking **"Build out the rest of your dashboard"** starts an in-place sequence (modal or
inline panel consistent with the existing design system):

1. **Beat 1 — priorities**: "What do you care about?" — the existing 7 priority cards
   (repurposed picker, same selection logic feeding `state.priorities` → header chips).
   Continue button.
2. **Beat 2 — email**: a single field: "Enter your email to generate your dashboard" +
   demo microcopy ("Demo — nothing is sent or stored."). Optional input; Enter/button
   proceeds either way; initials → user chip when provided.
3. **Beat 3 — build-out**: short "Generating your full dashboard" moment (reuse the
   step-checklist overlay pattern, 3–4 steps, ~2s), then the **six remaining tabs animate
   into the tab bar one-by-one** (staggered slide/fade, ~120ms apart), priorities chips
   appear in the header. User stays on the dashboard the whole time.
4. **Beat 4 — chat reveal**: after tabs land, the **"Consult Stagwell AI"** button
   appears bottom-right with an attention pulse (rename from "Ask Stagwell AI" on the FAB
   only; panel title can stay). It stays closed until clicked; clicking opens the
   existing panel (docked behavior on wide screens once opened is fine). All solve-pill /
   modal / footer-CTA chat hooks keep working — if chat is invoked before Beat 4
   (e.g. via a modal's "Request a demo"), just open it normally; the pulse intro is only
   for the staged reveal.
5. Expansion is one-way for the session; all four beats skip instantly under
   reduced-motion (tabs appear at once, no pulses). Deep-link buttons (`.goto`,
   solve-pill evidence rows) that target not-yet-built tabs must trigger the expansion
   prompt or be hidden pre-expansion — no dead buttons.

## Sprint E — Cleanup, QA, ship

1. Delete dead code: snapshot/priorities/signup `<section>`s + their renderers/CSS where
   no longer referenced (priorities card markup is reused in Beat 1), `renderSnapshot`,
   signup form handler, theme toggle, gen-bar, suggestion cards, removed nav links.
2. Update `GEN` configs/copy; keep all timings in the config.
3. Update `docs/CHANGES.md` with a "Feedback round 1" section (what changed and why).
4. QA (required, headless Chromium; `NODE_PATH=$(npm root -g)`,
   `PLAYWRIGHT_BROWSERS_PATH=/opt/pw-browsers`): full flow search→gen→starter→expand
   (all 4 beats)→every tab→company modal→chat; 1440 + 390; `reducedMotion:'reduce'`
   context completes the whole flow instantly; zero console/page errors; syntax-check the
   inline script with `node --check` after every assembly.
5. Ship: commit to `claude/stagwell-ai-onboarding-handoff-64g36a` and push
   (`git push -u origin <branch>`); deploy `npx -y vercel@latest deploy --prod --yes
   --token "$VERCEL_TOKEN"` from repo root; verify
   https://stagwell-ai-dashboard-demo.vercel.app serves the new build (curl for a marker
   string).

### Do NOT
- Re-add dark mode, the snapshot page, or the four suggestion cards.
- Touch `const DATA`, the data-series palette, status colors, chart guardrails, the
  Nike-only gate, the scripted-chat contract, or the Solutions modal system.
- Invent any number not present in `DATA` (map layers included).
- Add any network request (fonts, tiles, topojson — everything stays inline).
