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
