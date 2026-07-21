# CLAUDE DESIGN HANDOFF — Stagwell AI · Nike Intelligence Demo

**Your job:** make every part of this website look better. Elevate it from "good demo" to
"the flagship demo Stagwell shows a Fortune-100 CMO." You own visual design end to end —
typography, color refinement, layout, iconography, spacing, motion, dark mode, mobile —
inside the hard constraints in §2. You are not being asked to add features, change copy
meaningfully, or touch the data.

- **Live site:** https://stagwell-ai-dashboard-demo.vercel.app
- **Code:** `index.html` at repo root — the entire app is this one file (~200KB: CSS in one
  `<style>` block, data as `const DATA`, app JS after it).
- **Current-state screenshots:** `docs/design-audit/01…12*.jpg` — your "before" reference.
- **Run it:** open `index.html` in a browser. No build step, no server needed.

---

## 1. What this product is (60-second orientation)

A Stagwell AI demo with two halves:

1. **Onboarding flow** (pre-login): brand search ("What brand… do you want to understand?")
   → animated "Building your intelligence" generation overlay → free snapshot (3 insight
   cards) → priorities picker → signup → "Provisioning your dashboard" overlay.
2. **Dashboard** (the product): "NIKE — Brand & Competitive Intelligence", 7 tabs
   (Executive Overview · Social Competitive · Search & Keywords · AI Visibility ·
   Backlinks & Risk · **Stagwell Solutions** · Data Notes), a docked scripted
   "Ask Stagwell AI" chat panel, a light/dark theme toggle, and ~33 gold ✦ "solve pills"
   that deep-link metrics to Stagwell Marketing Cloud products on the Solutions tab.

Demo rules that shape design: only "Nike" searches work; the chat is scripted; nothing is
sent anywhere; results are deliberately *not* instant (skeletons, staged overlays,
count-ups, bars growing from zero).

---

## 2. HARD CONSTRAINTS — do not break these

1. **Single self-contained file.** Everything inline in `index.html`. No CDNs, no webfont
   requests, no external images, no network calls of any kind. (System-stack typography is
   a constraint to design *within* — see §4.1.)
2. **The 6-brand data-series palette is machine-validated for colorblind safety and is
   FROZEN** unless you re-validate: Nike `#2a78d6` / adidas `#eb6834` / PUMA `#1baf7a` /
   Under Armour `#eda100` / New Balance `#e87ba4` / lululemon `#008300` (dark variants in
   the tokens). Never reassign, cycle, or repaint per rank. If you must change any hex, run
   the six-checks validator (CVD ΔE ≥ 8 adjacent pairs, ≥3:1 contrast on both surfaces)
   and record the receipt in Data Notes. Sub-3:1 fills (PUMA/UA/NB on light) require the
   existing direct-label relief — keep every value directly labeled.
3. **Stagwell gold `#FFB81C` is a UI accent only — never a data series** (it would
   impersonate the warning status). **Status colors** (good `#0ca30c` / warn `#fab219` /
   serious `#ec835a` / crit `#d03b3b`) appear only where data means good/bad: toxicity,
   severity dots, deltas, alert tiles, AQS bands. Never as "series 7."
4. **Data integrity is sacred.** Every number comes from `const DATA` (mirrored at
   `docs/nike-dashboard-data.json`). Don't invent, round differently, or drop the
   honesty apparatus: 🔒 locked values, "showing N of {exact total}" truncation notes,
   the dashed 3-anchor traffic line, source tags on every card, "so what" annotation
   lines, provider boundary (IMAI vs SEMrush never cross-computed).
5. **One y-axis per chart. No donuts where stacked bars are specified. No linear per-brand
   follower bars** (the IG share strip exists for that reason). See
   `docs/NIKE-CHART-SPECS.md` §5 guardrails — they still apply to any restyle.
6. **Motion must honor `prefers-reduced-motion`** — the instant path must remain instant
   and complete (this is tested). Keep all timings in the existing `GEN` config / CSS
   custom props so they stay tunable.
7. **Behavioral contracts to preserve:** Nike-only search gate; tab switching preserves
   scroll; chat stays scripted with its "scripted demo" disclosure; theme toggle works both
   ways; the ✦ solve pills must keep navigating to and flash-highlighting their Solutions
   card; "Request a demo" opens the chat.
8. **Don't regress QA:** zero console errors; usable at 1440px (boardroom) and 390px
   (phone); both themes; print stylesheet stays functional.

---

## 3. How to work in this codebase

- `index.html` structure, top to bottom: `<style>` (tokens → chrome → onboarding →
  dashboard components → chat → solutions → motion/loading → media queries) → HTML
  skeleton (topnav, `#gen` overlay, 5 `<section>` screens, chat panel) → `<script>`
  (`const DATA` minified JSON → helpers/`GEN`/`SOLUTIONS` catalog → screen renderers →
  `tabExec/tabSocial/tabSearch/tabAI/tabLinks/tabSolutions/tabNotes` → chat).
- Charts are hand-rolled: horizontal bars/meters/stacks are HTML+CSS; scatter, dashed
  line, and sparklines are inline SVG. Chart HTML is produced by helpers
  (`barList`, `stack100`, `meterRow`, `kpiTiles`, `solvePill`) — restyling those helpers
  restyles most of the app at once. Prefer that over per-instance overrides.
- **QA loop (required):** headless Chromium via Playwright
  (`NODE_PATH=$(npm root -g)`, `PLAYWRIGHT_BROWSERS_PATH=/opt/pw-browsers`); walk
  search → snapshot → priorities → signup → each tab; screenshot 1440 & 390, light & dark,
  plus a `reducedMotion:'reduce'` context; assert no console/page errors. Look at the
  screenshots — label collisions and overflow are the defects that slip through.
- Deploy is Vercel CLI from repo root (`npx vercel deploy --prod --yes`) — coordinate with
  the repo owner, or leave deploying to them.

---

## 4. Current design system (your starting point)

All tokens live in `:root` and `html[data-theme="dark"]` at the top of the CSS.

| Group | Light | Dark |
|---|---|---|
| Surfaces | card `#ffffff` · alt `#faf9f6` · page `#f2f1ec` | `#14201f` · `#0f1918` · `#0a0f10` |
| Ink | `#0b1a20` / `#4c5257` / `#8a8f93` | `#f4f6f5` / `#b9c0c2` / `#7f8688` |
| Brand chrome | gold `#FFB81C` (+`#E5A200`), deep teal-navy `#04222E`/`#0a3341`, soft gold tint `#fff4d6`, link `#0A5E86` | gold `#FFC23D`, same deeps, link `#5fb8d6` |
| Elevation | 3-step shadow scale (`--shadow-sm/--shadow/--shadow-lg`) | deeper equivalents |
| Motion | `--ease: cubic-bezier(.22,.61,.36,1)`; keyframes: shimmer, fadeUp, growX/growY, pulseGlow, checkPop, barsweep, solflash | same |
| Radius | 16px cards, 12px tiles, 999px pills | same |

### 4.1 Typography (the biggest single upgrade opportunity)
Currently `"Helvetica Neue", system-ui, …` for everything, weights 400/600/700/800,
uppercase section headers with a 4px gold tick. It's clean but generic — nothing about the
type says "Stagwell." Within the no-webfont constraint you still have room: a deliberate
type scale (display/stat/label/body/caption with fixed sizes + line heights instead of
today's ad-hoc px values), stronger display treatment for hero numbers (`font-stretch`,
tighter negative tracking, `font-feature-settings`, tabular-nums everywhere numeric),
and better hierarchy between card title / so-what / source tag. If you conclude an
embedded font is worth it, a **subsetted, base64-inlined WOFF2** (one weight, latin
subset, ≤40KB) is acceptable within the single-file rule — document the size cost.

### 4.2 Known visual debts — an honest list (fix these)
1. **Emoji as icons everywhere** (🏢👥👟🎯 suggestion cards, 📣🧑‍🤝‍🧑 priorities, ☁️🤖📊 on
   Solutions, ⛔⚠▲ severity, 🔒 locks, ✦ pills). Emoji render differently per OS and read
   as prototype-grade. Replace with a single consistent inline-SVG icon set (stroke-based,
   1.5–2px, currentColor) — this is likely the highest-leverage polish in the whole app.
2. **No brand mark / favicon / meta.** Text-only "STAGWELL AI" logotype; no favicon,
   no `<meta name="theme-color">`, no OG/description tags. Design a simple S-mark or
   gold-square mark (inline SVG + data-URI favicon), add the meta set.
3. **Chat panel dead space** — a tall empty gap between the greeting and the suggestion
   chips (see `12-chat-panel.jpg`). Needs a designed empty state (mini capability cards,
   a subtle watermark, or suggestions anchored under the greeting).
4. **Footer CTA repeats identically on all 7 tabs** — same dark band, same copy. Vary or
   slim it; it reads as boilerplate by tab three.
5. **Scoreboard table density** — 18 rows × 7 cols with best/worst tints + ▲▼ works but is
   visually noisy; the nikecol left-border treatment is subtle to a fault. Consider row
   grouping (reach / engagement / quality / economics), sticky first column on mobile,
   and a calmer best/worst treatment.
6. **Brand profile cards** are uneven heights with raw text rows; the `<details>` audience
   expander is bare. Grid alignment, consistent slots, and a designed expander would help.
7. **Solutions cards**: the mini-cards (QuestRQ, Unlock, ID Graph) sit awkwardly in the
   same grid as full cards (see `09-dash-solutions.jpg` — orphaned single-card rows);
   the "FOR NIKE" block could be a stronger moment; contact emails overflow-wrap poorly
   at narrow widths.
8. **Onboarding screens 2–4** (snapshot/priorities/signup) are plain white pages with
   centered content — they lack the branded atmosphere of the search hero and the
   generation overlay. Below-the-fold whitespace on the snapshot screen is dead.
9. **In-bar labels on 100% stacks** hide below 12% width (data lives in the legend/title) —
   fine functionally, but small segments look mute; consider leader-line labels or
   end-labels for key segments.
10. **Hand-tuned scatter label offsets** in the quality map (`labOff` per brand) — fragile;
    a smarter collision-avoiding placement (or callout lines) would look better and
    survive data changes.
11. **Gender split colors borrow brand-slot hues** (NB magenta for Female, blue-300 for
    Male) — legal per legend/labels but semantically muddy on a page full of brand colors.
    A dedicated, validated F/M pair would read cleaner (validate it).
12. **KPI tiles**: value sizes jump between 40px and 28px via a `.smaller` class; delta
    chips sometimes wrap under the sub-label; alert tile styling (left border) and hover
    top-border gold compete.
13. **Dark-mode seams**: warning-tint backgrounds and `.loser`/`.best` table tints were
    tuned for light and go murky in dark; the gen overlay and hero are permanently dark
    (fine) but check every callout/tint pair in dark.
14. **Mobile (390px)**: works, but the 7-tab strip scrolls with no affordance/fade hint,
    KPI tiles stack into a long single column, tables rely on horizontal scroll with no
    edge hint, and the chat FAB can cover CTAs (see `11-mobile-390.jpg`).
15. **Print stylesheet** is minimal (hides chrome, breaks-inside avoid) — a designed
    one-pager print/PDF of the Executive tab would be a genuinely useful artifact.
16. **Skeletons are generic** (gray tiles/one chart shape) and don't match the layouts
    they replace — per-tab skeletons that mirror real geometry sell the "generating"
    illusion much harder.
17. **Count-up + entrance animations race full-page screenshots** — cosmetic only, but if
    you rework motion, keep total first-paint choreography under ~1.2s per tab.

---

## 5. Screen-by-screen briefs

Reference the numbered screenshots in `docs/design-audit/`.

| # | Screen / component | Keep | Make better |
|---|---|---|---|
| 01 | **Search hero** | Full-viewport dark hero, gold accents, coach mark, Nike-only notice | More atmosphere (subtle texture/gradient motion, product-name marquee, sample-query ghost typing); real icons on suggestion cards; design the notice as a component, not a strip |
| 02 | **Generation overlay** | Step choreography, weighted timing, progress bar | Brand mark instead of ✦ square; richer step iconography; consider log-style microcopy ("291.7M followers found…") — must stay data-true; exit transition into the snapshot |
| 03 | **Free snapshot** | 3-card structure, so-what lines, source tags | Give the page the hero's dark-to-light atmosphere; cards deserve mini-visualization polish; CTA row hierarchy; kill dead space below fold |
| — | **Priorities & signup** | Selection interaction, demo-only note | Same atmosphere treatment; selected-card states beyond a border; signup form styling is stock — design fields, focus, and the delivery-prefs chips |
| 04 | **Executive tab** | KPI row + red flags + share strip + ER/growth + sparklines; solve pills | This is the 30-second read — tighten vertical rhythm so it fits closer to one viewport at 1440; red-flag list is long (severity icon + title + detail + two link rows per item — consolidate); sparkline tiles are plain |
| 05 | **Social tab** | Scoreboard, platform-shade bars, quality map, likers section, brand cards | See debts 5/6/10/11; the likers callout is a key story — give it presence; consider a visual divider system between the 6 chart rows |
| 06 | **Search tab** | Intent stacks, movers diverging bars, gap-by-rival, full-report chips | Longest tab — needs sectioning (sub-headers or grouped bands: Demand / Gaps / Pages / Health); `<details>` expanders (pages 2–3, subdomains) are unstyled |
| 07 | **AI tab** | Warning callout (the CMO takeaway), per-engine cards, engine colors | The callout is the money moment — make it feel like an alert from the product, not a styled div; per-engine cards could carry engine wordmark-style headers (text only) |
| 08 | **Backlinks tab** | Toxicity stack, audit strip, AS distribution, anchors detail | Most table-dense tab; the monospace offender list could be a designed "threat list"; vertical AS bars need axis polish |
| 09 | **Solutions tab** | Hero band, category grouping, FOR-NIKE blocks, contacts, flash highlight | The sales surface — worth the most love. Product-mark treatment (consistent 2-letter marks feel placeholder), grid balance for mini-cards, hover states, maybe a compact "which product for which metric" matrix at top |
| 10 | **Dark mode** | Token-based stepped palette | Audit every tint/violet seam (debt 13); dark is what they'll demo on a boardroom screen — treat it as first-class |
| 11 | **Mobile 390** | Everything stacks, FAB chat | Debt 14; also check gen overlay, solutions grid, and scoreboard at 390 |
| 12 | **Chat panel** | Scripted flow, chips, solve links, disclosure | Debt 3; message bubbles, typing indicator, and the chip row all deserve the brand treatment; "scripted demo" label styled as a proper badge |
| — | **Data Notes tab** | Complete honesty content | It's a wall of `<ol>` text — design it like release notes / a methodology page (grouped cards, lock table styling) |

---

## 6. Definition of done

1. Every screenshot in `docs/design-audit/` has a visibly better "after," at 1440 and 390,
   light and dark — regenerate the folder with your afters when finished.
2. All §2 constraints verified intact: palette receipts (if any hex changed), data
   apparatus untouched, reduced-motion instant path, zero console errors, tabs fit with
   chat docked at 1440, solve pills still navigate + flash.
3. No emoji remain as UI iconography (chat message content may keep them).
4. Favicon + theme-color + title/meta present.
5. The QA walkthrough script (§3) passes; note anything you deliberately changed about
   motion timing in a short CHANGES section appended to this file.

---

*Prepared by the previous build session. Product/data context: `NIKE-DASHBOARD-HANDOFF.md`
(build spec) · `NIKE-CHART-SPECS.md` (chart rules + §6 round-2 specs) ·
`nike-dashboard-data.json` (single source of truth) · `NIKE-CMO-MASTER-REPORT.md`
(narrative) · `STAGWELL-PRODUCT-ONE-PAGERS.md` (Solutions-tab source). The Stagwell brand
book was never provided — chrome colors were reconstructed from public assets (gold
`#FFB81C`, deep blue, white); if a brand book arrives, exact hexes/typeface supersede.*
