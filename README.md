# Stagwell AI — Nike Intelligence Dashboard (Demo)

A single-file, self-contained demo of the Stagwell AI product experience:
the pre-login onboarding flow from the *Stagwell AI Success* vision doc,
landing on the **Nike CMO Brand & Competitive Intelligence dashboard**
built to the v2 spec package (`docs/NIKE-DASHBOARD-HANDOFF.md` +
`docs/NIKE-CHART-SPECS.md` + `docs/nike-dashboard-data.json`).

## Run it

Open `index.html` in any browser — or deploy the repo to Vercel as a static
site (no build step, no config needed; `vercel.json` just enables clean URLs).
No CDN, no network calls; all charts are hand-rolled SVG/CSS, so everything
works fully offline.

## The flow

1. **Brand search** — “What brand, product, audience, or market do you want
   to understand?” *Demo gate: only Nike is wired up; any other query shows
   a friendly notice.*
2. **Free snapshot** — three insight cards from public data, value before signup.
3. **Priorities** — “What would make this useful to you?” selectable signal
   cards (carried onto the dashboard as chips).
4. **Sign up** — name/email/role + delivery preferences. *Demo only — nothing
   is sent or stored.*
5. **Dashboard** — 6 tabs (Executive Overview · Social Competitive ·
   Search & Keywords · AI Visibility · Backlinks & Risk · Data Notes) with a
   persistent **Ask Stagwell AI** chat panel and a **light/dark theme toggle**.

## Built to spec (v2 package)

- **Dataset embedded verbatim:** `docs/nike-dashboard-data.json` is inlined
  as `const DATA` — no transcription; display strings (`d` fields) render as-is.
- **Charts per `NIKE-CHART-SPECS.md`:** E1–E6, S0–S7, K0–K8, A0–A4, B1–B8 —
  including the IG **share strip** (no linear follower bars), the LLM
  **stacked bar** (no donut), the dashed 3-anchor traffic line, and the
  emphasis-mode “who AI cites” callout.
- **Validated palette:** the CVD-validated 6-brand categorical palette
  (Nike `#2a78d6` … Lululemon `#008300`), light and dark steps; **volt is a
  UI accent only**, status colors reserved for good/warning/serious/critical;
  sub-3:1 fills always carry direct labels (relief rule).
- **Dark mode** is a stepped dark palette (selected, not an invert).
- **Data integrity:** provider boundaries kept (IMAI vs SEMrush, never
  cross-computed), locked values render 🔒 with the required unlock named,
  truncated tables state “showing N of {exact total}”.

## Demo constraints (by design)

- **Nike-only:** the search gate intentionally rejects other brands.
- **Scripted AI chat:** the chat panel replays canned, source-cited answers
  drawn from the dataset — it is not connected to a live model.

## Files

| Path | What it is |
|---|---|
| `index.html` | The entire demo app (onboarding + dashboard + chat) |
| `docs/nike-dashboard-data.json` | Machine-readable dataset (single source of truth) |
| `docs/NIKE-CHART-SPECS.md` | Per-chart build spec (forms, palette, guardrails) |
| `docs/NIKE-DASHBOARD-HANDOFF.md` | Build instructions / definition of done |
| `docs/NIKE-CMO-MASTER-REPORT.md` | Human-readable narrative + provenance |
| `docs/NIKE-DATA-COMPLETION-PROMPT.md` | Round-2 scrape prompt (for a browser-equipped agent; not executed here) |
| `vercel.json`, `.vercelignore` | Static Vercel deployment config |
