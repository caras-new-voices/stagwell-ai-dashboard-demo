# PROMPT: Complete the Nike CMO Intelligence Dataset (Round 2 Scrape + Merge)

Give this prompt to an agent with browser-automation tools (Claude with the Chrome plugin), in a browser logged into SEMrush (project `fid=12722449`). The working folder is `Nike Stagwell AI`, which contains the existing package.

---

## 1. WHAT HAPPENED BEFORE YOU (context — read first)

On 21 July 2026 a previous session built a CMO-level competitive intelligence package for Nike in two scraping passes plus a consolidation:

**Pass 1 — Social (InfluencerMarketing.AI / imai.co).** Full Instagram-first brand reports were extracted for Nike and 5 competitors (adidas, PUMA, Under Armour, New Balance, lululemon) via URLs of the form `https://imai.co/reports;platform=instagram;socialId={handle}`. Captured: profile, per-platform followers, engagement/post, post/story/reels economics, EMV, IMAI scores, followers breakdown, full audience demographics, hashtags/mentions, trend ranges, top 2 posts per brand.

**Pass 2 — Search/Web (SEMrush, project fid=12722449, nike.com).** Captured: SEO Dashboard widgets, Domain Overview (Worldwide), Organic Research positions + competitors (page 1 each), Backlink Analytics overview, AI Visibility overview, Keyword Gap vs footlocker.com (page 1), Traffic Analytics summary, Position Tracking / Site Audit / Backlink Audit summaries. Advertising Research and deep Traffic Analytics hit paywalls (not purchased).

**Consolidation.** Everything was merged into a 4-file build package for a dashboard:

| File (in this folder) | Role |
|---|---|
| `nike-dashboard-data.json` | Machine-readable dataset — **the file you will be updating** |
| `NIKE-CHART-SPECS.md` | Per-chart build specs (IDs E1–E6, S0–S7, K0–K8, A0–A4, B1–B8) |
| `NIKE-DASHBOARD-HANDOFF.md` | Build instructions for the dashboard |
| `NIKE-CMO-MASTER-REPORT.md` | Human-readable consolidated narrative |

**The problem you are solving:** an audit found the dataset is complete at summary level but truncated at row level — big tables were captured at page 1 only, several IMAI sub-sections were never expanded, and some SEMrush tools were never opened. Your job is to capture the missing data and merge it into `nike-dashboard-data.json` without breaking anything that's already there.

## 2. GROUND RULES (unchanged from round 1)

1. **Read-only.** Never click Export/Download/CSV, "Set up", Share, or anything that changes account state or spends credits. Read on-screen tables only.
2. **No purchases, no trials, no credentials.** If a paywall appears, record it as locked and move on.
3. **Never fabricate.** Empty/gated → record `null` or `"LOCKED"` with the reason.
4. **Exact values**, verbatim formatting, with the context block (URL, db, device, date) noted per capture.
5. Pages lazy-load — wait for spinners ("Measuring engagement stats...", skeletons) before extracting; retry once after 8s if content is missing.

## 3. THE GAP LIST — what to capture, in priority order

### Tier A — IMAI (no login observed; reports load publicly)
URL pattern: `https://imai.co/reports;platform=instagram;socialId={handle}` for handles: `nike`, `adidas`, `puma`, `underarmour`, `newbalance`, `lululemon`.

1. **Posts 3–10 per brand** (currently only 2 of 10). Scroll to the Posts section; also click the sub-tabs **Popular / Commercial Posts / Recent Posts / Recent Reels / Top Reels** and capture each list: date, caption, likes, comments per post.
2. **"View more" expansions** — the report has View more links on: Followers/Following/Likes trends (may reveal monthly data points), Engagements spread, Popular #/@, Gender split, Age & gender, Location by city/country, Ethnicity, Language, Brand affinity, Interest affinity, Audience lookalikes, Top followers, Lookalikes. Click each; capture whatever extra rows/points appear.
3. **Likers tab** — Audience data has a Followers/Likers toggle; we only captured Followers. Toggle to **Likers** and capture the same demographic tables (gender, age, geo, ethnicity, language, affinity).
4. **Audience tab & Posts tab** — the report body has Influencer / Audience / Posts tabs; we mostly read Influencer. Open the other two fully.
5. Note-but-don't-fight: TikTok/YouTube sub-tabs and Brand Safety scores are locked (🔒) — confirm and move on.

### Tier B — SEMrush deeper pages (logged-in, included in plan)
Confirmed working URL patterns (swap params as noted):

1. **Organic keywords pages 2–5** — `https://www.semrush.com/analytics/organic/positions/?db=us&q=nike.com&searchType=domain&fid=12722449` → use the table's Next/page control for pages 2–5 (~500 rows total). Columns: keyword, intent, position, SF, traffic, traffic %, volume, KD%, URL, updated.
2. **Position Changes tab** — same tool, sub-tab "Position Changes": New / Improved / Declined / Lost counts + top rows of each.
3. **Pages & Subdomains tabs** — top pages by traffic (URL, traffic, keywords) and subdomain split.
4. **Organic Rankings for UK** (`db=uk`) — headline metrics + top 20 keywords (we have US only; UK = 749.6K keywords).
5. **Backlink Analytics sub-tabs** — Backlinks (top ~50 rows: source, target, anchor, AS, type, first seen), Anchors (top ~30), Indexed Pages (top ~20): base `https://www.semrush.com/analytics/backlinks/` + `backlinks/`, `anchors/`, `indexed/` with `?q=nike.com&searchType=domain&fid=12722449`.
6. **Keyword Gap vs remaining rivals** — rerun `https://www.semrush.com/analytics/keywordgap/?q=nike.com&fid=12722449&searchType=domain&rankType=common&db=us` adding each competitor in the second slot (type domain + Enter): `adidas.com`, `dickssportinggoods.com`, `stockx.com`. Capture: overlap counts, segment breakdown, top-10 opportunities each.
7. **Backlink Gap** — left ☰ menu → Backlink Gap; nike.com vs footlocker.com + adidas.com: prospect domains table (top ~30).
8. **Compare Domains** — `https://www.semrush.com/analytics/comparedomains/?db=us&device=desktop&currency=usd&q=nike.com&searchType=domain&compareWith=footlocker.com:domain|hibbett.com:domain|champssports.com:domain|flightclub.com:domain&fid=12722449` — the side-by-side metric matrix.
9. **Domain Overview: Growth report + Compare by countries sub-tabs**, and the **US country tab** (we captured Worldwide detail + US headline only).
10. **AI Visibility per-engine views** — append `&llm=gpt`, `&llm=aiOverview`, `&llm=aiMode`, `&llm=gemini` to `https://www.semrush.com/ai-seo/overview/?db=worldwide&q=nike.com&fid=12722449`; capture per-engine metrics + top topics. Also `&preset=brandedSources` variant, and the "Cited Pages" list if it renders.
11. **Site Audit full report + Position Tracking full report** — from the SEO Dashboard (`https://www.semrush.com/seo/30511459/?fid=12722449`) click each "View full report": issue list (name, severity, # pages) and full tracked-keyword table (~87 keywords with positions).
12. **Backlink Audit full report** — toxic-link table beyond the 7 shown (top ~50: source URL, toxicity score, markers).

### Tier C — confirm still locked (record, don't attempt)
Advertising Research (Advertising Toolkit paywall), Traffic Analytics channel %/geo/pages (Traffic & Market Toolkit Pro paywall), IMAI TikTok/YouTube tabs + Brand Safety. If any now renders data, capture it — plans change.

## 4. HOW TO MERGE INTO THE PACKAGE

Work on `nike-dashboard-data.json` (schemas documented in `_meta._schemas`; follow existing conventions: numeric `v` + display `d`, pct as plain numbers, `null` for absent, `"LOCKED"` for gated).

1. **Append, don't rewrite.** Extend existing arrays (`search.topKeywordsUS.rows` grows from 42 toward ~500) and update their `note` fields ("Page 1 of 41,804" → "Pages 1–5 of 41,804 (~500 of 4,180,334)").
2. **New keys for new datasets**, following existing naming:
   - `social.brands.{brand}.allPosts` (schema `post`, tagged by sub-tab), `social.brands.{brand}.audienceLikers` (same shape as `audience`), extended `trends` if monthly points appear (`points: [[month, value], …]` — only real values, never interpolations).
   - `search.positionChanges`, `search.topPages`, `search.subdomains`, `search.ukOverview`, `search.keywordGapVsAdidas` / `VsDicks` / `VsStockx` (same shape as `keywordGapVsFootlocker`), `search.backlinkGap`, `search.compareDomains`, `search.siteAuditIssues`, `search.positionTrackingKeywords`.
   - `backlinks.topBacklinks`, `backlinks.anchorsDetail`, `backlinks.indexedPages`, `backlinks.audit.toxicLinks`.
   - `ai.byEngineDetail.{gpt|aiOverview|aiMode|gemini}`.
3. **Validate after every major merge:** `node -e "require('./nike-dashboard-data.json')"` must pass; spot-check 5 pre-existing values are unchanged.
4. **Update the companions:**
   - `NIKE-CMO-MASTER-REPORT.md`: append a "Round 2 additions" section summarizing new data; update Part 6 caveats (what's no longer truncated).
   - `NIKE-CHART-SPECS.md`: update table footers ("showing N of TOTAL"); if a new dataset earns a new chart (e.g., trend lines from real monthly points replacing the dashed 3-anchor line in K6, a Likers-vs-Followers comparison, per-engine AI panels), add specs following the existing format — form chosen by data job, brand palette unchanged (it is CVD-validated; do not add colors), status colors reserved, direct-label relief rule maintained.
   - `nike-dashboard-data.json._meta`: bump `snapshotDate` scheme to `{"round1":"2026-07-21","round2":"<your date>"}` and note that rounds may differ — round-2 numbers must NOT be averaged with round-1 numbers; if a headline metric changed, keep both with dates.
5. **Update `locked[]`** — remove anything you captured, keep anything still gated, with the paywall name/price you observed.

## 5. REPORT BACK

When done, deliver: the updated 4 files, plus a short changelog: rows added per table, new datasets captured, anything that failed or was still locked (with screenshots' worth of description), and any number that changed between rounds. Do not build the dashboard — that's a separate task that consumes this package.

Begin with Tier A brand 1 (`socialId=nike`), and work the list in order.
