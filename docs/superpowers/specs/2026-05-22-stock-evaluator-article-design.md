# Article Design Spec: Stock Evaluator — How to Use It as a Screener

**Date:** 2026-05-22
**Slug:** `stock-evaluator-guide`
**Estimated read time:** 6–7 min
**Target audience:** Investors already familiar with fundamental analysis metrics (PE, ROE, D/E etc.) who want to understand how Buffett, Dalio, and Graham frameworks differ and how to use the tool as a shortlisting filter.

---

## Core Message

The Stock Evaluator is a screener, not a buy signal. It runs three established investment frameworks against live data to help you shortlist stocks worth researching further — and its limitations are as instructive as its scores.

---

## Article Structure

### Section 1 — Hook (≈120 words)

Open with a concrete conflict scenario: run a stock through all three frameworks and get three different verdicts. Buffett says Buy. Graham says Avoid. Dalio says Partial Fit. This is not a bug — it's a feature. The three frameworks were built by investors with different goals, different eras, different risk tolerances. The tool surfaces that tension deliberately. This intro hooks the target reader (someone who knows enough to find that interesting) without requiring any background explanation.

No CTA here. Let the tension breathe.

---

### Section 2 — Warren Buffett Framework (≈250 words)

**Philosophy sentence:** Buffett looks for wonderful businesses at fair prices — quality-first, with the patience to hold forever.

**Key criteria to explain with "why":**

| Criterion | Why it matters |
|---|---|
| Return on Equity > 15% | Measures how efficiently management compounds capital — Buffett's central proxy for business quality |
| Earnings Growth > 5% | A business that isn't growing earnings is slowly losing ground to inflation |
| Debt/Equity < 0.5 | Low debt = the company doesn't need to be rescued in a downturn; it survives on its own terms |

**Lighter-touch criteria (mention, don't dwell):** PE < 20x, pays dividend > 1%, positive profit margin, forward PE < trailing PE.

**Verdict labels:** Strong Buy / Buy / Hold / Avoid (ratio-based, not absolute)

**Scoring note:** D/E is skipped (not failed) for financial sector companies — structurally high leverage is normal for banks and insurers.

**CTA:** Inline link → Stock Evaluator after this section.

---

### Section 3 — Ray Dalio Framework (≈200 words)

**Philosophy sentence:** Dalio is an all-weather investor — he designs portfolios to survive any economic environment, not maximise returns in a bull market.

**Key criteria to explain with "why":**

| Criterion | Why it matters |
|---|---|
| Beta < 1.0 | Low volatility = portfolio diversifier; Dalio prizes uncorrelated assets that don't move with the market |
| Current Ratio > 1.5 | Liquidity to survive downturns — a Dalio core principle is that debt crises always hit the illiquid first |
| Profit Margin > 5% | Pricing power signals a company can pass inflation through to customers |

**Lighter-touch criteria (mention):** Positive revenue growth, D/E < 1.0 (skipped for financials), ROE > 10%.

**Verdict labels:** Strong Fit / Good Fit / Partial Fit / Poor Fit

**Note on contrast:** Dalio's D/E threshold (< 1.0) is deliberately more lenient than Buffett's (< 0.5) — he's looking for resilience, not purity.

**CTA:** Inline link → Stock Evaluator after this section.

---

### Section 4 — Benjamin Graham Framework (≈250 words)

**Philosophy sentence:** Graham is the father of value investing — he wanted a margin of safety so wide that even if he was wrong about the business, he'd still make money.

**Key criteria to explain with "why":**

| Criterion | Why it matters |
|---|---|
| P/E < 15 | Graham's strict ceiling — he rejected the idea of paying a premium for growth; the price had to leave room for error |
| P/B < 1.5 | Buying close to book value means you're not paying much above what the assets are actually worth — classic margin of safety |
| Current Ratio ≥ 2 | Graham required very strong liquidity — his experience of the Depression shaped this; he'd seen solvent businesses fail because they couldn't meet short-term obligations |
| Market Cap ≥ $2B | He avoided small companies for stability reasons; size implies some resilience and public scrutiny |

**Lighter-touch criteria (mention):** Positive EPS, pays dividend, D/E < 0.5 (skipped for financials).

**Why Graham will often say Avoid:** His thresholds are strict by design. Most modern companies — especially tech — won't pass P/B < 1.5. That's not a flaw; it's the filter working as intended. Graham would have held cash rather than buy most of the S&P 500 today.

**CTA:** Inline link → Stock Evaluator after this section.

---

### Section 5 — How to Use the Combined Score (≈150 words)

Short practical bridge. The tool shows a combined score (Buffett + Dalio + Graham, max 20) with a colour chip:
- Green: ≥ 13
- Amber: ≥ 8
- Red: < 8

The useful workflow: run a shortlist of 5–10 candidates. Look for stocks that pass 2 of 3 frameworks — not unanimously, but with broad agreement. Then investigate the ones that fail: *why* does Graham hate it? Is it because it's a high-quality compounder trading above book (Buffett territory) or because it's actually overpriced?

The score is a conversation starter, not a conclusion.

---

### Section 6 — Limitations (≈250 words)

Two categories — clearly separated:

**Framework limitations (what no ratio can capture):**
- **Economic moat:** The durability of a competitive advantage — brand, network effects, switching costs, cost advantages. None of the three frameworks directly tests for moat. A company can pass all 20 criteria and have no moat.
- **Management quality:** Buffett says he'd rather own a great business run by average management than an average business run by great management — but neither the business quality nor the management quality appears in a balance sheet.
- **Industry tailwinds and disruption:** A retailer passing all Dalio criteria in 2010 could still be Kodak. Structural disruption doesn't show up in historical ratios until it's too late.
- **Framework age:** Graham's thresholds (P/E < 15, P/B < 1.5) were calibrated against mid-20th century markets. Applying them to today's intangible-heavy tech companies produces almost universal "Avoid" verdicts — which may say more about the framework's era than the stock's quality.

**Data limitations (Yahoo Finance):**
- Data is delayed ~15 minutes — not suitable for timing decisions.
- Some fields are unavailable for smaller or less-covered companies (earnings growth, forward PE, dividend yield). When data is missing, the criterion is skipped — the score adjusts to reflect only available data.
- Yahoo Finance has no SLA. Data can be stale, missing, or occasionally incorrect. Treat every number as a starting point for verification, not a final source.

---

### Section 7 — Closing (≈80 words)

Brief close: the tool is most useful when you treat the frameworks as lenses rather than verdicts. Each one highlights something different — Buffett finds quality, Graham finds value, Dalio finds resilience. A stock that passes all three is rare and worth looking at hard. A stock that fails all three probably deserves to stay on the shelf.

**CTA 1:** Run a stock through the evaluator → Stock Evaluator link

**CTA 2:** Once you've shortlisted candidates, check if your portfolio is diversified → Portfolio Health Check link

---

## Technical / Publishing Details

- **Slug:** `stock-evaluator-guide`
- **Path:** `www/stock-evaluator-guide/index.html`
- **Date:** 2026-05-22
- **Byline:** KashVector · 22 May 2026 · 6 min read
- **Article JSON-LD schema:** Article type
- **FAQPage JSON-LD schema:** 4 questions (see below)
- **No deploy until author reviews locally**

### FAQ questions for JSON-LD + FAQ section

1. Is the Stock Evaluator free to use?
2. Which stock markets does it support?
3. Does the tool give buy or sell recommendations?
4. How current is the data?

---

## What This Article Is Not

- Not a tutorial on how to use the UI (no screenshots, no "click here")
- Not a biography of Buffett/Dalio/Graham
- Not financial advice
