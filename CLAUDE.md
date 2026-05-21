# CLAUDE.md

This file provides guidance to Claude Code when working in this repository.

## What this repo is

Source for the KashVector articles section. Vanilla HTML/CSS — no build step. Articles are authored here and deployed by copying `www/` into `C:\Projects\StockAnalysis\www\articles\`.

## Structure

```
www/
  article.css                        # Shared styles for all article pages
  index.html                         # Articles listing page (/articles/)
  2026-budget-what-changes/
    index.html                       # Article page (/articles/2026-budget-what-changes/)
docs/superpowers/
  specs/2026-05-19-kashvector-articles-design.md   # Design spec
  plans/2026-05-19-kashvector-articles.md          # Implementation plan
```

## Deploy workflow

1. Copy source to the StockAnalysis hub repo:
   ```bash
   cp -r www/. "C:/Projects/StockAnalysis/www/articles/"
   ```
2. Add new article URLs to `C:\Projects\StockAnalysis\www\sitemap.xml`
3. Commit and push from `C:\Projects\StockAnalysis` — Cloudflare Pages auto-deploys

**Production URL:** `https://kashvector.com/articles/`

## Adding a new article

1. Create `www/<slug>/index.html` — copy the existing article as a template
2. Add an entry to `www/index.html` (the listing page)
3. Update `kv-banner.js` in **both** `C:\Projects\StockAnalysis\www\kv-banner.js` **and** `C:\Projects\Budget Planner\public\kv-banner.js` — change `LATEST.title` and `LATEST.href` to point to the new article
4. Add the new article URL to the sitemap
5. Update **all three** "From the blog" placements on `C:\Projects\StockAnalysis\www\index.html` (hub landing page):
   - `#blog-sidebar` — fixed left sidebar shown on desktop (≥1360px); add new `<a>` block above existing links, before "All articles →"
   - `#from-the-blog` — inline section shown on mobile/narrow screens; add new `<div style="margin-top:10px;">` above existing entries
   - `header-article-link` — hardcoded "🔥 Hot article:" pill inside `<header>`; update the `href` and link text to the new article. This is separate from `kv-banner.js` and only appears on the landing page.
   - Newest article first in all placements
6. Deploy (step above)

## Design system

Articles use `article.css` for all styling. Key CSS classes:

| Class | Purpose |
|-------|---------|
| `.article-body` | Max-width 720px article container |
| `.article-headline` | H1 — 1.75rem, bold |
| `.article-byline` | Author · date · read time |
| `.article-summary` | Highlighted summary box |
| `.article-cta` | Bordered link card pointing to a KashVector tool |
| `.article-cta-label` | Small uppercase label above the CTA text |
| `.article-cta-text` | Main CTA text in accent colour |
| `.article-disclaimer` | Italic legal disclaimer at bottom |
| `.faq-item` | `<details>` accordion in the FAQ section |

Dark mode is default (`html.dark`). Light mode via `html:not(.dark)`. CSS custom properties (`--kv-*`) are defined in `article.css` for local dev but the live site inherits them from `/brand.css`.

## Page structure (every article)

```html
<head>
  <script src="/kv-theme.js"></script>   <!-- must be first — prevents flash -->
  <link rel="stylesheet" href="/brand.css">
  <link rel="stylesheet" href="../article.css">
  <!-- Article + FAQPage JSON-LD schemas -->
</head>
<body>
  <header class="app-header">...</header>
  <script src="/kv-nav.js"></script>
  <script src="/kv-banner.js"></script>
  <main>
    <article class="article-body">
      <div class="article-breadcrumb"><a href="/articles/">← Articles</a></div>
      <!-- content -->
    </article>
    <section class="tool-info">
      <!-- FAQ <details> items -->
    </section>
  </main>
  <footer>...</footer>
</body>
```

## KashVector tool URLs (for CTAs)

| Tool | URL |
|------|-----|
| Budget Planner | `/budget/` |
| FIRE Calculator | `/fire/` |
| Debt Recycling / Investment Property | `/debt-recycling/` |
| Budget Impact Calculator | `/budget-impact/` |
| Mortgage Calculator | `/mortgage/` |
| Salary Sacrifice Calculator | `/super-compare/` |
| Rent vs Buy vs Rentvest | `/rent-vs-buy/` |
| Portfolio Health Check | `/portfolio-health/` |
| Stock Evaluator | `/stock/` |
| Life Buyback Calculator | `/life-buyback/` |
