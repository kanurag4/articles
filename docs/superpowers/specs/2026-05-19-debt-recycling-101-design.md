# Debt Recycling 101 — Article Design Spec

Date: 2026-05-19
Slug: `debt-recycling-101`
Output: `www/debt-recycling-101/index.html`

---

## Purpose

Introduce debt recycling to Australian homeowners who have heard the term but don't know how it works. Targets beginners (the "101" framing), covering the core mechanism and all three strategies available in the KashVector Debt Recycling Calculator.

---

## Target Reader

An Australian homeowner with a mortgage, possibly an offset account, looking to understand whether debt recycling could accelerate their wealth. Not yet an investor — wants plain English before deciding to dig deeper.

---

## Structure

### Head matter
- Title: "Debt Recycling 101: How to Turn Your Mortgage Into a Wealth-Building Tool"
- Meta description: ~150 chars targeting "what is debt recycling Australia"
- JSON-LD: Article + FAQPage schemas
- Read time: ~7 min

### Summary box (`.article-summary`)
Three bullets:
- Debt recycling converts non-deductible home loan debt into tax-deductible investment debt
- There are three strategies — offset, equity → shares, equity → property
- The ATO allows it, but structure and direct purpose matter

### Section 1 — What is debt recycling?
The core problem: home loan interest isn't tax-deductible, but investment loan interest is. Debt recycling bridges the gap by ensuring borrowed funds are used for income-producing investments. ATO requirement stated plainly: direct link between borrowed funds and income-producing asset. Separate loan split or account makes this audit-proof.

### Section 2 — Why it works: the tax engine
Concrete example at 32% marginal rate: $100k at 6% = $6,000 interest → $1,920 ATO deduction. Effective rate drops from 6% to ~4.1%. Spread over long-run index return of 7–8% is where wealth gain compounds.

### Section 3 — Strategy 1: Offset account
Deep coverage. Step-by-step mechanism (4 steps). Net borrowing stays the same — offset buffer replaced by invested assets. Monthly recycling loop explained. Pros: no new debt, simpler to start. Trade-off: giving up offset certainty for market variability + tax benefit. CTA → Debt Recycling Calculator.

### Section 4 — Strategy 2: Equity release → Stocks/ETFs
Deep coverage. Net borrowing INCREASES — this is new debt against the property. Mechanism explained with numbers (LVR, accessible equity). Risk/reward framed honestly: higher total debt, but historically indexed ETFs held 10+ years have rewarded that risk. Suits: stable income, long horizon, can tolerate volatility. CTA → Debt Recycling Calculator.

### Section 5 — Strategy 3: Equity → Property
Short (2–3 paragraphs). Same equity-release mechanism but buying an IP instead. Negative gearing restriction from 1 July 2027 significantly changes the maths for established properties purchased after 12 May 2026. New builds exempt. CGT also changed for post-budget assets. Framed as "still viable in some scenarios, but check the numbers." CTA → Budget Impact Calculator + Debt Recycling Calculator (property tab).

### Section 6 — Is debt recycling right for you?
Four prerequisite questions:
1. Do you have a home loan?
2. Do you have an offset balance or accessible equity?
3. Is your income stable?
4. Can you hold for the long term (10+ years)?
No recommendation made. Ends with: "the numbers are real, but so are the risks."

### Disclaimer
Standard general-information-only, recommend licensed adviser.

### FAQ section (`.tool-info` + `.faq-item`)
4 items:
1. Is debt recycling legal in Australia?
2. What does the ATO require for the interest deduction to be valid?
3. Does the 2026 budget affect the offset or equity-to-shares strategies?
4. Is this article financial advice?

---

## CTAs (in order of appearance)

| Section | Tool | URL |
|---|---|---|
| After offset strategy | Debt Recycling Calculator | `/debt-recycling/` |
| After equity → shares | Debt Recycling Calculator | `/debt-recycling/` |
| After equity → property | Budget Impact Calculator | `/budget-impact/` |
| After equity → property | Debt Recycling Calculator | `/debt-recycling/` |

---

## Key Accuracy Points

- **Offset strategy:** net borrowing unchanged. Offset balance moves from cash buffer to invested assets.
- **Equity release:** net borrowing increases. This is new leverage — must be stated clearly.
- **Equity → property post-12 May 2026:** negative gearing quarantined for established properties from 1 July 2027. New builds exempt.
- **CGT:** post-budget assets use indexation + 30% minimum rate, not 50% discount.
- **ATO requirement:** direct purpose link between borrowed funds and income-producing investment. Separate loan split is best practice.

---

## What this article is NOT

- Not a how-to implementation guide (refer reader to a financial adviser for structuring)
- Not specific financial advice
- Does not make a buy/don't-buy recommendation on any strategy
