# Design Spec: Auction Clearance Rates Article

**Date:** 2026-05-26
**Slug:** `auction-clearance-rates-2026`
**Title:** Is the 2026 Federal Budget Leading to Property Decline?

---

## Purpose

Examine whether the 2026 Federal Budget caused the decline in NSW auction clearance rates, or whether the decline was already underway before May 12. The article takes no predetermined side — it builds the argument from data, acknowledges limitations, and reaches a provisional conclusion. It does not sensationalise.

**Target reader:** Property owners and investors who have seen media headlines attributing the clearance rate slide to the budget and want a data-informed second opinion.

---

## Thesis

The media narrative attributes falling auction clearance rates to the 2026 Federal Budget. A 15-month analysis of Sydney clearance rate data across two primary reporting sources (Domain and CoreLogic) — overlaid with RBA cash rate decisions — suggests the decline predates the budget by at least 10 weeks, coinciding instead with the RBA's three consecutive rate hikes in February, March, and May 2026. The budget's measurable effect, if any, appears to be stabilisation of an already-falling market rather than a new leg down. However, with only two post-budget weekends of data available at time of writing, this conclusion is preliminary.

---

## Article Structure

### 1. Introduction (~150 words)
- Open with the question the article answers: did the budget cause the property market to weaken?
- Acknowledge the media coverage since May 12
- State the methodology upfront: 15 months of weekly clearance rate data, two reporting sources, overlaid with macro policy markers
- Include an explicit caveat: only two post-budget weekends exist at time of writing — conclusions are provisional

### 2. Understanding the Data: Why Different Sources Show Different Numbers (~200 words)
- Explain the Domain vs CoreLogic preliminary vs CoreLogic final methodology split before any data is shown
- Domain: agent-reported, counts withdrawals as failures, skews slightly lower
- CoreLogic preliminary: phone-poll model on weekend, winners report quickly → higher baseline (often 6–8pp above final)
- CoreLogic final/revised: released mid-week after late-reporting agents submit → lower, considered more accurate
- Purpose: inoculate the reader against dismissing chart data because "my number was different"
- This section makes the three-line chart legible before the reader sees it

### 3. Chart 1 — 15-Month Clearance Rate Trend
**Type:** Multi-line Chart.js, rendered in `<canvas>`

**Data:**
- Line 1 (blue, solid): Domain weekly Sydney clearance rate
- Line 2 (green, dashed): CoreLogic preliminary weekly
- Line 3 (red, dotted): CoreLogic final/revised weekly
- X-axis: weekly dates, Feb 2025 → May 2026
- Y-axis: clearance rate %

**Vertical annotation markers (rendered as vertical lines + labels):**
- Feb 2025: RBA cut → 4.10%
- May 2025: RBA cut → 3.85%
- Aug 2025: RBA cut → 3.60%
- Feb 2026: RBA hike → 3.85%
- Mar 2026: RBA hike → 4.10%
- May 5 2026: RBA hike → 4.35%
- May 12 2026: Federal Budget (distinct colour, dashed)

**Key visual story:** The decline slope begins at the Feb 2026 hike marker. The budget marker sits 10+ weeks into an already-established downtrend.

**Caption:** Cite Domain and CoreLogic (Cotality) as sources. Note preliminary vs final distinction.

### 4. Section: The Pre-Budget Trend (~250 words)
- Walk through the three-act chronology neutrally:
  - Act 1 (Feb–Aug 2025): Three successive RBA cuts drove market from ~65% to peak clearance in Jan 2026 (~75–78%)
  - Act 2 (Feb–Apr 2026): Three hikes reversed the rally; Domain clearance fell from ~76% (Feb 7) to ~49% (week of Apr 25 — ANZAC weekend)
  - By May 12: Market was already at its weakest level since 2022
- Present this as chronological observation, not editorial judgment
- Reference Chart 1 throughout

### 5. Chart 2 — Week-on-Week Slope (Velocity)
**Type:** Bar or line chart showing weekly percentage-point change

**Data:**
- One series per source (Domain, CoreLogic prelim, CoreLogic final)
- Y-axis: pp change (positive = market gaining, negative = market falling)
- Same vertical markers as Chart 1

**Key visual story:** The sharpest negative bars fall at the Feb and Mar 2026 hike dates. The budget marker shows slope flattening toward zero — not accelerating downward.

**Caption:** Explain that slope = week-on-week pp change. A slope of 0 = no change; negative = market weakening.

### 6. The Two Post-Budget Weekends (~200 words)
- Report the actual data plainly, with source attribution:
  - Week of May 10–16 (first post-budget weekend): Domain ~49%, CoreLogic preliminary ~57%
  - Week of May 17–23: Domain ~54%, CoreLogic preliminary ~64.2%
- Slope moved from approximately -2pp → near-flat → slight positive
- Discuss two interpretations neutrally:
  - Interpretation A: Budget provided psychological reassurance, arresting the freefall
  - Interpretation B: The market was due for a technical bounce after sustained overselling
- State clearly: two data points cannot distinguish between these interpretations

### 7. Methodology Limitations (own section, ~150 words)
- **Sample size:** Two post-budget weekends is statistically insufficient to isolate cause and effect
- **Lag effect:** Auction listings committed 2–4 weeks before the auction date — clearance rates in the first two post-budget weekends partly reflect pre-budget sentiment
- **Preliminary revision gap:** CoreLogic preliminary figures are revised materially by mid-week; using preliminary data overstates the rate
- **Seasonality:** May auction volumes differ from summer peaks; volume affects clearance rates independently of sentiment
- **Confounding variables:** The May 5 RBA hike occurred 7 days before the budget — disentangling their effects is not possible from clearance rate data alone
- **Correlation ≠ causation:** Coincidence of RBA hike dates with rate declines does not prove the hikes caused the declines

### 8. Conclusion (~100 words)
- Summarise the provisional finding: the available data does not support the narrative that the budget caused the clearance rate decline; the decline predates the budget by 10+ weeks and aligns more closely with the RBA hike cycle
- Acknowledge: the budget may have contributed to stabilisation (slope flattening), but this cannot yet be confirmed
- Commit to monitoring: the article will be updated as further post-budget weekends accumulate
- Avoid definitive language — this is a live question with limited data

### 9. CTA
- Tool: Mortgage Calculator (`/mortgage/`)
- Text: "See how rate changes affect your repayments"

---

## Charts: Technical Spec

**Library:** Chart.js 4.x via CDN (`https://cdn.jsdelivr.net/npm/chart.js`)

**Chart 1 config:**
- Type: `line`
- Three datasets (Domain, CL prelim, CL final)
- Point radius: 2, borderWidth: 2
- Annotation plugin for vertical lines (use `chartjs-plugin-annotation` via CDN)
- Tooltip: show all three values on hover
- Responsive: true, aspect ratio ~2.5 on desktop

**Chart 2 config:**
- Type: `bar` (grouped, one bar group per week)
- Same annotation markers as Chart 1
- Zero-line prominent (borderDash on grid at y=0)
- Colour: red bars below 0, green bars above 0 (use per-point backgroundColor array)

**Colours (dark mode compatible):**
- Domain: `#38bdf8` (KashVector accent blue)
- CoreLogic prelim: `#4ade80` (green)
- CoreLogic final: `#f87171` (red)
- RBA cut markers: `#4ade80`
- RBA hike markers: `#f87171`
- Budget marker: `#facc15` (yellow, distinct)

---

## Data Sources

**To be verified and locked during implementation:**

RBA decisions (confirmed):
- Feb 18 2025: cut to 4.10%
- May 20 2025: cut to 3.85%
- Aug 2025: cut to 3.60%
- Feb 3 2026: hike to 3.85%
- Mar 2026: hike to 4.10%
- May 5 2026: hike to 4.35%

Known clearance rate data points (Domain/CoreLogic — to be supplemented during implementation):
- Feb 2025: CoreLogic prelim ~76.6%
- Jun 2025: CoreLogic ~73.5%
- Nov 2025: CoreLogic ~56.1%
- Jan 31 2026: Domain ~78%, CoreLogic prelim ~76%
- Feb 7 2026: Domain ~76%
- Mar 7 2026: Domain ~63%
- Mar 14 2026: Domain ~61%
- Mar 21 2026: Domain ~57%
- Apr 11 2026: Domain ~54%
- Apr 25 2026 (ANZAC): Domain ~49%
- May 3–9 2026: Domain ~54%
- May 10–16 2026: Domain ~49%, CoreLogic prelim ~57% (first post-budget weekend)
- May 17–23 2026: Domain ~54%, CoreLogic prelim ~64.2%

**Sources to cite in article:**
- Domain auction results: domain.com.au/auction-results/sydney/
- CoreLogic/Cotality weekly reports: corelogic.com.au/news-research/
- RBA cash rate history: rba.gov.au/statistics/cash-rate/
- PropertyUpdate weekly reports: propertyupdate.com.au

---

## Tone Guidelines

- Professional, analytical — reads like a research note, not a blog post
- No superlatives ("smashed", "crushed", "surged")
- Attribute all data to named sources
- Acknowledge what the data cannot tell us, not just what it can
- Provisional language for post-budget conclusions: "suggests", "appears to", "at this stage"

---

## Page Structure

Follows standard KashVector article template:
- `kv-theme.js` first in `<head>`
- `/brand.css` + `../article.css`
- Article + FAQPage JSON-LD schema
- `kv-nav.js` + `kv-banner.js` after `<header>`
- `.article-breadcrumb` → H1 → byline → ai-note → content
- FAQ section reuses `.faq-item` / `<details>` pattern
- AI authorship note (`.article-ai-note`) per site standard

---

## FAQ Topics (for JSON-LD + `<details>` accordion)

1. What is an auction clearance rate and how is it calculated?
2. Why do Domain and CoreLogic report different clearance rates?
3. Did the 2026 Federal Budget directly change property tax rules?
4. How many RBA rate decisions have there been since February 2025?
5. Is this article financial advice?
