# Stagwell AI — Nike Intelligence Dashboard (Demo)

A single-file, self-contained demo of the Stagwell AI product experience:
the pre-login onboarding flow from the *Stagwell AI Success* vision doc,
landing on the **Nike CMO Brand & Competitive Intelligence dashboard**
specified in `docs/NIKE-DASHBOARD-HANDOFF.md`.

## Run it

Open `index.html` in any browser. No build step, no server, no network —
all CSS/JS is inline and every chart is hand-rolled SVG/CSS, so it works
fully offline.

## The flow

1. **Brand search** — “What brand, product, audience, or market do you want
   to understand?” *Demo gate: only Nike is wired up; any other query shows
   a friendly notice.*
2. **Free snapshot** — three insight cards from public data, value before
   signup.
3. **Priorities** — “What would make this useful to you?” selectable signal
   cards (carried onto the dashboard as chips).
4. **Sign up** — name/email/role + delivery preferences. *Demo only —
   nothing is sent or stored.*
5. **Dashboard** — 6 tabs: Executive Overview · Social Competitive ·
   Search & Keywords · AI Visibility · Backlinks & Risk · Data Notes,
   with a persistent **Ask Stagwell AI** chat panel.

## Demo constraints (by design)

- **Nike-only:** the search gate intentionally rejects other brands.
- **Scripted AI chat:** the chat panel replays canned, source-cited answers
  drawn from the master report — it is not connected to a live model.
- **Data integrity:** every number comes verbatim from
  `docs/NIKE-CMO-MASTER-REPORT.md` (snapshot 20–21 Jul 2026, IMAI + SEMrush).
  Locked/unknown values render as 🔒 with the required upsell named; sampled
  tables state “showing N of {exact total}”; IMAI and SEMrush figures are
  never cross-computed.

## Files

| Path | What it is |
|---|---|
| `index.html` | The entire demo app (onboarding + dashboard + chat) |
| `docs/NIKE-CMO-MASTER-REPORT.md` | Single source of truth for every number |
| `docs/NIKE-DASHBOARD-HANDOFF.md` | The build spec this implements |
