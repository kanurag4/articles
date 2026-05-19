# KashVector Articles Section — Design Spec
Date: 2026-05-19

## Overview

Add a `/articles/` section to kashvector.com and publish the first article: "2026 Federal Budget: What Changes for Everyday Australians". The goal is to build organic traffic and content depth to support a future Commission Factory affiliate application.

---

## Goals

- Give KashVector a discoverable, crawlable articles section
- Publish one high-value, time-sensitive article while the 2026 budget is fresh news
- Drive tool usage naturally within the article (Budget Planner, Budget Impact Calculator)
- Pass Commission Factory's content and traffic requirements on reapplication

---

## Scope

**In scope:**
- `kv-banner.js` — site-wide injected banner linking to the latest article
- Articles listing page (`kashvector.com/articles/`)
- First article page (`kashvector.com/articles/2026-budget-what-changes/`)
- "From the blog" section on the hub landing page
- Banner added to all vanilla tool pages and Budget Planner

**Out of scope:**
- CMS, build pipeline, markdown conversion
- Pagination, search, comments, social sharing
- Second article (scope for later)

---

## Repositories

| Role | Path |
|---|---|
| Source | `C:\Projects\Articles\` |
| Deploy target | `C:\Projects\StockAnalysis\www\articles\` |
| Shared site assets | `C:\Projects\StockAnalysis\www\` (`kv-banner.js` lives here) |

**Deploy workflow:**
```bash
cp -r "C:/Projects/Articles/www/." "C:/Projects/StockAnalysis/www/articles/"
# commit & push StockAnalysis → Cloudflare auto-deploys
```

---

## File Structure

### New files

```
C:\Projects\Articles\
  www/
    index.html                        ← articles listing page
    article.css                       ← shared styles for listing + article pages
    2026-budget-what-changes/
      index.html                      ← first article

C:\Projects\StockAnalysis\www\
  kv-banner.js                        ← site-wide banner script (new)
```

### Modified files

| File | Change |
|---|---|
| `StockAnalysis\www\index.html` | Add banner script tag + "From the blog" section |
| `StockAnalysis\www\<each-tool>\index.html` | Add `<script src="/kv-banner.js"></script>` (10 tool pages) |
| `Budget Planner\public\kv-banner.js` | Copy of kv-banner.js for React build |
| `Budget Planner\index.html` | Add `<script src="/kv-banner.js"></script>` |

---

## Component: kv-banner.js

Site-wide injected slim banner. Same pattern as `kv-nav.js`.

**Behaviour:**
- IIFE — no global pollution
- Injects a `<div class="kv-article-banner">` immediately after `<nav class="kv-tool-nav">`
- MutationObserver fallback for React (Budget Planner) where the nav renders asynchronously
- Double-injection guard: checks for `.kv-article-banner` before injecting
- Single config object at the top — update `title` and `href` when a new article is published:

```js
const LATEST = {
  title: '2026 Federal Budget — What Changes for Everyday Australians',
  href: '/articles/2026-budget-what-changes/'
};
```

**Visual design:**
- Slim strip (~40px), full width
- Background: `--kv-card` / `#1e293b`
- Left border: 3px solid `--kv-accent` / `#38bdf8`
- Text: `New article:` (muted) + article title (text, sky-accent on hover) + `→`
- Font size: `0.82rem`, same as the cross-tool nav
- Light mode: background `#d4dce8`, border `#0ea5e9`
- Injected CSS is inlined in the script (same approach as kv-nav.js)

---

## Articles Listing Page (`/articles/index.html`)

**Header:** standard KashVector header — logo, `← All tools` home link, theme toggle, cross-tool nav, banner

**Body:**
- `<h1>Articles</h1>` with subtitle: "Plain-English guides to Australian personal finance"
- Article list: single column, `max-width: 720px`, centred
- Each entry: title (link), date (muted), 2-line description, `Read →`

**SEO:**
- `<title>Articles — KashVector</title>`
- `<meta name="description">` — ~150 chars describing the articles section
- `WebApplication` JSON-LD in `<head>`
- Entry added to `www/sitemap.xml`

---

## Article Page (`/articles/2026-budget-what-changes/index.html`)

**Header:** same standard KashVector header + nav + banner

**Body layout:** single column, `max-width: 720px`, centred, comfortable reading width

### Article content structure

| Section | Content |
|---|---|
| Headline | "2026 Federal Budget: What Changes for Everyday Australians" |
| Byline | "KashVector · 19 May 2026 · 6 min read" |
| Hook (2–3 sentences) | Warm opener — "Every year the budget lands and every year you wonder if any of it actually affects you. This year, it does." |
| Summary bullets | 3 plain-English bullets — one per change, no numbers yet |
| Section 1 | Your tax bill is going down |
| Section 2 | Buying an investment property? The rules just changed |
| Section 3 | Capital gains tax is being overhauled |
| Closing | 2 sentences + final CTA |
| Disclaimer | "This is general information, not financial advice." |

### Section 1 — Your tax bill is going down

**Coverage:**
- First bracket drops from 16% → 15% from 1 July 2026 (then 14% from 1 July 2027)
- Upper thresholds raised: $120k → $135k and $180k → $190k
- New $1,000 instant work expense deduction (no receipts needed) from 2026–27
- Concrete dollar example: average earner ($81k) gets ~$268/year more from 1 July 2026

**CTA card:** "See your take-home pay → Budget Planner" (`/budget/`)

### Section 2 — Thinking of buying an investment property? The rules just changed

**Coverage:**
- From 1 July 2027, established properties purchased after 12 May 2026 can no longer offset rental losses against salary
- Losses are quarantined — carried forward to offset future rental income or capital gain at sale (not lost forever)
- Who is grandfathered: existing investors (bought before 12 May 2026) — no change
- Who is exempt: new builds — still fully deductible
- Plain-English impact: "If your property costs you $500/month more than it earns in rent, you used to be able to reduce your tax bill by that loss. Under the new rules, you can't — at least not immediately."

**CTA card:** "Model the impact on your property → Budget Impact Calculator" (`/budget-impact/`)

### Section 3 — Capital gains tax is being overhauled

**Coverage:**
- For assets acquired after 12 May 2026, the 50% CGT discount is gone
- Replaced by: inflation indexation of the cost base + 30% minimum tax rate on the indexed gain
- When the new rules are better: low real growth (inflation erodes most of the nominal gain)
- When the old rules are better: high real growth (large gains, 50% discount was very valuable)
- Grandfathering: assets bought before 12 May 2026 keep the 50% discount until 1 July 2027, then a split-gain rule applies

**CTA card:** "Compare old vs new CGT rules → Budget Impact Calculator" (`/budget-impact/`)

### Closing

Two sentences acknowledging the changes don't all hit at once, but the time to model is now. Final CTA to Budget Impact Calculator.

### FAQ + About section

Standard KashVector FAQ section before footer. 4 FAQ items:
1. When do these changes actually start?
2. Does negative gearing affect my existing investment property?
3. What is the capital gains tax discount and why is it changing?
4. Is this general advice?

---

## Hub Landing Page Changes (`www/index.html`)

### Slim banner (A)
- Added immediately after `<nav class="kv-tool-nav">` via `kv-banner.js`
- Same visual design as described in kv-banner.js section above

### "From the blog" section (C)
- Static HTML added below the tool grid, above the footer
- Heading: "From the blog"
- One article preview card: title, date, one-liner, `Read →` link to `/articles/2026-budget-what-changes/`
- Card styled with slate card background and sky-accent hover border — distinct from tool cards
- Scales to 3-column grid on desktop as more articles are added
- Uses hardcoded hex values (no CSS vars — consistent with rest of hub page)

---

## Tone & Style

- **Audience:** engaged but non-expert Australians — comfortable with terms like "negative gearing" and "capital gains" but not looking for a tax lecture
- **Tone:** warm, plain-spoken — contractions, "you", short sentences, like a knowledgeable friend explaining it over coffee
- **CTAs:** feel helpful, not salesy — every CTA follows naturally from the section content
- **Length:** ~1,200–1,400 words

---

## Shared CSS (`article.css`)

Covers:
- Article body typography (line height, paragraph spacing, heading sizes)
- Inline CTA cards (sky-accent border, card background, hover state)
- Article list entries on the index page
- "From the blog" preview cards (used in hub page inline styles override)
- Byline and date styles
- Disclaimer style (muted, small, italic)
- FAQ section (reuses existing `.tool-info` / `.faq-item` pattern)

---

## SEO

| Page | Title | Description |
|---|---|---|
| `/articles/` | `Articles — KashVector` | Plain-English guides to Australian tax, super, property, and personal finance. Free, no sign-up. |
| `/articles/2026-budget-what-changes/` | `2026 Federal Budget: What Changes for Everyday Australians — KashVector` | The 2026 budget cuts income tax, restricts negative gearing, and overhauls capital gains tax. Here's what it means for you in plain English. |

- `WebApplication` JSON-LD on both pages
- `FAQPage` JSON-LD on the article page (from the 4 FAQ items)
- Article page added to `www/sitemap.xml`
- Articles index added to `www/sitemap.xml`

---

## Updating the Banner (future articles)

When a new article is published:
1. Edit `kv-banner.js` in `StockAnalysis\www\` — update `LATEST.title` and `LATEST.href`
2. Copy to `Budget Planner\public\kv-banner.js` and rebuild Budget Planner
3. Add new article entry to `Articles\www\index.html` (the articles listing page)
4. Add new preview card to hub "From the blog" section
5. Push StockAnalysis → Cloudflare auto-deploys

---

## Out of Scope (future work)

- Second and subsequent articles
- Article tagging / filtering
- RSS feed
- Email newsletter
- Social sharing buttons
- Analytics per-article tracking
