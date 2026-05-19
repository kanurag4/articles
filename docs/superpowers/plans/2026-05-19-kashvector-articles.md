# KashVector Articles Section Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a `/articles/` section on kashvector.com, ship a site-wide article banner across all tools, and publish the first article: "2026 Federal Budget: What Changes for Everyday Australians."

**Architecture:** Three repos involved — `C:\Projects\Articles` (source for article pages), `C:\Projects\StockAnalysis` (deploy target + shared scripts), `C:\Projects\Budget Planner` (React app, gets a copy of the banner). The articles source is a static vanilla JS project matching existing KashVector conventions. Shared scripts (`kv-banner.js`) live in StockAnalysis alongside `kv-nav.js` and `kv-theme.js`.

**Tech Stack:** Vanilla HTML/CSS/JS, no build step, no framework. Same stack as all other KashVector tools except Budget Planner.

---

## File Map

### New files
```
C:\Projects\Articles\
  www\
    index.html                              ← articles listing page
    article.css                             ← shared styles (listing + article pages)
    2026-budget-what-changes\
      index.html                            ← first article

C:\Projects\StockAnalysis\www\
  kv-banner.js                              ← site-wide banner (new shared script)
```

### Modified files
```
C:\Projects\StockAnalysis\www\
  index.html                                ← add banner script + "From the blog" section
  stock\index.html                          ← add kv-banner.js script tag
  fire\index.html                           ← add kv-banner.js script tag
  mortgage\index.html                       ← add kv-banner.js script tag
  debt-recycling\index.html                 ← add kv-banner.js script tag
  portfolio-health\index.html               ← add kv-banner.js script tag
  super-compare\index.html                  ← add kv-banner.js script tag
  life-buyback\index.html                   ← add kv-banner.js script tag
  rent-vs-buy\index.html                    ← add kv-banner.js script tag
  budget-impact\index.html                  ← add kv-banner.js script tag

C:\Projects\Budget Planner\
  public\kv-banner.js                       ← copy of kv-banner.js for Vite build
  index.html                                ← add kv-banner.js script tag
```

---

## Local Dev Note

`kv-nav.js`, `kv-banner.js`, `brand.css`, and `kv-theme.js` are served from the StockAnalysis root (`/`). When developing the Articles project in isolation with `npx http-server www -p 8001`, these scripts will 404 — that's expected. The nav and banner only work on the deployed site. Verify the article layout locally, then verify nav/banner integration on the live deployed URL after pushing.

---

## Task 1: Initialize the Articles project and write article.css

**Files:**
- Create: `C:\Projects\Articles\www\index.html` (placeholder — filled in Task 3)
- Create: `C:\Projects\Articles\www\article.css`
- Create: `C:\Projects\Articles\www\2026-budget-what-changes\index.html` (placeholder — filled in Task 4)

- [ ] **Step 1: Create the folder structure**

```bash
cd "C:\Projects\Articles"
mkdir -p www/2026-budget-what-changes
```

- [ ] **Step 2: Git init**

```bash
cd "C:\Projects\Articles"
git init
echo "node_modules/" > .gitignore
```

- [ ] **Step 3: Write article.css**

Create `C:\Projects\Articles\www\article.css`:

```css
/* ── Shared KashVector vars (mirrors brand.css for standalone dev) ── */
:root {
  --kv-bg:      #0f172a;
  --kv-card:    #1e293b;
  --kv-card-2:  #273449;
  --kv-text:    #f1f5f9;
  --kv-muted:   #94a3b8;
  --kv-accent:  #38bdf8;
  --kv-accent-h:#0ea5e9;
  --kv-border:  #334155;
  --kv-pass:    #22c55e;
}

/* ── Light mode ── */
html:not(.dark) {
  --kv-bg:     #edf2f7;
  --kv-card:   #d4dce8;
  --kv-card-2: #c5cfe0;
  --kv-text:   #0f172a;
  --kv-muted:  #64748b;
  --kv-border: #e2e8f0;
}

* { box-sizing: border-box; margin: 0; padding: 0; }

body {
  background: var(--kv-bg);
  color: var(--kv-text);
  font-family: system-ui, -apple-system, "Segoe UI", Roboto, sans-serif;
  font-size: 15px;
  min-height: 100vh;
}

/* ── Header ── */
.app-header {
  position: relative;
  text-align: center;
  padding: 28px 20px 20px;
  border-bottom: 1px solid var(--kv-border);
}

.app-header h1 {
  font-size: 1.4rem;
  font-weight: 700;
  color: var(--kv-text);
}

.app-header p {
  font-size: 0.85rem;
  color: var(--kv-muted);
  margin-top: 4px;
}

.home-logo {
  position: absolute;
  top: 12px;
  left: 16px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 3px;
  text-decoration: none;
}

.home-logo img { width: 32px; height: 32px; opacity: 0.9; display: block; transition: opacity 0.15s; }
.home-logo:hover img { opacity: 1; }
.home-logo-label {
  font-size: 0.6rem;
  font-weight: 500;
  letter-spacing: 0.03em;
  color: var(--kv-muted);
  transition: color 0.15s;
  white-space: nowrap;
}
.home-logo:hover .home-logo-label { color: var(--kv-accent); }

.theme-toggle {
  position: absolute;
  top: 16px;
  right: 16px;
  width: 32px;
  height: 32px;
  border-radius: 50%;
  border: 1px solid var(--kv-border);
  background: var(--kv-card);
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--kv-muted);
  font-size: 0.9rem;
  transition: background 0.15s, border-color 0.15s;
}
.theme-toggle:hover { background: var(--kv-card-2); border-color: var(--kv-accent); color: var(--kv-text); }

/* ── Articles listing page ── */
.articles-list {
  max-width: 720px;
  margin: 0 auto;
  padding: 48px 20px 80px;
}

.articles-list-title {
  font-size: 1.5rem;
  font-weight: 700;
  color: var(--kv-text);
  margin-bottom: 6px;
}

.articles-list-subtitle {
  font-size: 0.9rem;
  color: var(--kv-muted);
  margin-bottom: 40px;
}

.article-entry {
  border-bottom: 1px solid var(--kv-border);
  padding: 20px 0;
}

.article-entry:last-child { border-bottom: none; }

.article-entry-title {
  font-size: 1.05rem;
  font-weight: 600;
  margin-bottom: 4px;
}

.article-entry-title a {
  color: var(--kv-text);
  text-decoration: none;
  transition: color 0.15s;
}

.article-entry-title a:hover { color: var(--kv-accent); }

.article-entry-date {
  font-size: 0.78rem;
  color: var(--kv-muted);
  margin-bottom: 6px;
}

.article-entry-desc {
  font-size: 0.88rem;
  color: var(--kv-muted);
  line-height: 1.5;
  margin-bottom: 8px;
}

.article-entry-read {
  font-size: 0.82rem;
  color: var(--kv-accent);
  text-decoration: none;
  font-weight: 500;
}

.article-entry-read:hover { color: var(--kv-accent-h); }

/* ── Article page body ── */
.article-body {
  max-width: 720px;
  margin: 0 auto;
  padding: 40px 20px 80px;
}

.article-breadcrumb {
  font-size: 0.8rem;
  color: var(--kv-muted);
  margin-bottom: 20px;
}

.article-breadcrumb a {
  color: var(--kv-muted);
  text-decoration: none;
  transition: color 0.15s;
}

.article-breadcrumb a:hover { color: var(--kv-accent); }

.article-headline {
  font-size: 1.75rem;
  font-weight: 700;
  line-height: 1.3;
  color: var(--kv-text);
  margin-bottom: 12px;
}

.article-byline {
  font-size: 0.82rem;
  color: var(--kv-muted);
  margin-bottom: 28px;
  padding-bottom: 24px;
  border-bottom: 1px solid var(--kv-border);
}

.article-body p {
  font-size: 0.95rem;
  line-height: 1.75;
  color: var(--kv-text);
  margin-bottom: 16px;
}

.article-body h2 {
  font-size: 1.15rem;
  font-weight: 600;
  color: var(--kv-text);
  margin: 36px 0 12px;
  padding-bottom: 8px;
  border-bottom: 1px solid var(--kv-border);
}

.article-body ul {
  padding-left: 20px;
  margin-bottom: 16px;
}

.article-body li {
  font-size: 0.95rem;
  line-height: 1.7;
  color: var(--kv-text);
  margin-bottom: 6px;
}

.article-summary {
  background: var(--kv-card);
  border: 1px solid var(--kv-border);
  border-radius: 10px;
  padding: 18px 22px;
  margin-bottom: 28px;
}

.article-summary p {
  font-size: 0.85rem;
  font-weight: 600;
  color: var(--kv-muted);
  margin-bottom: 10px;
}

.article-summary ul { margin: 0; }
.article-summary li { font-size: 0.9rem; color: var(--kv-muted); }

/* ── CTA card ── */
.article-cta {
  display: block;
  background: var(--kv-card);
  border: 1px solid var(--kv-accent);
  border-radius: 10px;
  padding: 16px 20px;
  margin: 24px 0 32px;
  text-decoration: none;
  transition: background 0.15s, border-color 0.15s;
}

.article-cta:hover { background: var(--kv-card-2); border-color: var(--kv-accent-h); }

.article-cta-label {
  display: block;
  font-size: 0.72rem;
  color: var(--kv-muted);
  margin-bottom: 4px;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.article-cta-text {
  font-size: 0.95rem;
  font-weight: 600;
  color: var(--kv-accent);
}

/* ── Disclaimer ── */
.article-disclaimer {
  font-size: 0.8rem;
  color: var(--kv-muted);
  font-style: italic;
  margin-top: 32px;
  padding-top: 20px;
  border-top: 1px solid var(--kv-border);
  line-height: 1.6;
}

/* ── FAQ section (reuses KashVector pattern) ── */
.tool-info {
  max-width: 720px;
  margin: 0 auto;
  padding: 0 20px 48px;
}

.tool-info h2 {
  font-size: 1rem;
  font-weight: 600;
  color: var(--kv-text);
  margin-bottom: 16px;
  padding-bottom: 8px;
  border-bottom: 1px solid var(--kv-border);
}

.faq-item { border-bottom: 1px solid var(--kv-border); padding: 10px 0; }
.faq-item summary {
  font-size: 0.9rem;
  font-weight: 500;
  color: var(--kv-text);
  cursor: pointer;
  list-style: none;
  padding-right: 20px;
  position: relative;
}
.faq-item summary::-webkit-details-marker { display: none; }
.faq-item summary::after { content: '+'; position: absolute; right: 0; color: var(--kv-muted); }
.faq-item[open] summary::after { content: '−'; }
.faq-item p { font-size: 0.85rem; color: var(--kv-muted); line-height: 1.6; margin-top: 8px; }

/* ── Footer ── */
footer {
  text-align: center;
  padding: 24px 20px;
  border-top: 1px solid var(--kv-border);
  font-size: 0.8rem;
  color: var(--kv-muted);
}

footer a { color: var(--kv-muted); text-decoration: none; }
footer a:hover { color: var(--kv-accent); }

/* ── Mobile ── */
@media (max-width: 680px) {
  .app-header { display: flex; align-items: flex-start; gap: 8px; padding: 12px 16px; text-align: left; }
  .home-logo { position: static; flex: 0 0 auto; order: 1; padding-top: 4px; }
  .app-header-center { flex: 1; min-width: 0; order: 2; text-align: center; }
  .theme-toggle { position: static; flex: 0 0 auto; order: 3; width: 44px; height: 44px; }
  .app-header h1 { font-size: 1.1rem; }
  .article-headline { font-size: 1.3rem; }
  input, select, textarea { font-size: 16px; }
}
```

- [ ] **Step 4: Create placeholder files**

Create `C:\Projects\Articles\www\index.html`:
```html
<!DOCTYPE html><html><head><title>Articles — KashVector</title></head><body><p>Coming soon.</p></body></html>
```

Create `C:\Projects\Articles\www\2026-budget-what-changes\index.html`:
```html
<!DOCTYPE html><html><head><title>Article — KashVector</title></head><body><p>Coming soon.</p></body></html>
```

- [ ] **Step 5: Verify folder structure**

```bash
ls "C:\Projects\Articles\www"
# Expected: article.css  index.html  2026-budget-what-changes/
ls "C:\Projects\Articles\www\2026-budget-what-changes"
# Expected: index.html
```

- [ ] **Step 6: Start local server and verify CSS loads**

```bash
cd "C:\Projects\Articles"
npx --yes http-server www -p 8001 -c-1
```

Open `http://localhost:8001`. Expect: "Coming soon." on a white page (kv-theme.js not loading locally — that's fine). Open DevTools → Network tab, confirm `article.css` returns 200.

- [ ] **Step 7: Initial commit**

```bash
cd "C:\Projects\Articles"
git add www/article.css www/index.html "www/2026-budget-what-changes/index.html" .gitignore
git commit -m "chore: initialize Articles project with article.css"
```

---

## Task 2: Write kv-banner.js

**Files:**
- Create: `C:\Projects\StockAnalysis\www\kv-banner.js`

- [ ] **Step 1: Write kv-banner.js**

Create `C:\Projects\StockAnalysis\www\kv-banner.js`:

```js
(function () {
  // Update LATEST when publishing a new article.
  const LATEST = {
    title: '2026 Federal Budget — What Changes for Everyday Australians',
    href: '/articles/2026-budget-what-changes/'
  };

  const style = document.createElement('style');
  style.textContent = `
    .kv-article-banner {
      display: flex;
      align-items: center;
      gap: 6px;
      padding: 0 16px;
      height: 40px;
      background: #1e293b;
      border-left: 3px solid #38bdf8;
      border-bottom: 1px solid #334155;
      font-size: 0.82rem;
      overflow: hidden;
    }
    .kv-article-banner-label {
      color: #94a3b8;
      flex-shrink: 0;
      white-space: nowrap;
    }
    .kv-article-banner a {
      color: #f1f5f9;
      text-decoration: none;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      transition: color 0.15s;
    }
    .kv-article-banner a:hover { color: #38bdf8; }
    html:not(.dark) .kv-article-banner {
      background: #d4dce8;
      border-left-color: #0ea5e9;
      border-bottom-color: #e2e8f0;
    }
    html:not(.dark) .kv-article-banner-label { color: #64748b; }
    html:not(.dark) .kv-article-banner a { color: #0f172a; }
    html:not(.dark) .kv-article-banner a:hover { color: #0ea5e9; }
    @media (max-width: 600px) {
      .kv-article-banner { font-size: 0.75rem; padding: 0 12px; }
    }
  `;
  document.head.appendChild(style);

  function buildBanner() {
    const div = document.createElement('div');
    div.className = 'kv-article-banner';
    div.innerHTML =
      '<span class="kv-article-banner-label">New article:</span>' +
      '<a href="' + LATEST.href + '">' + LATEST.title + ' →</a>';
    return div;
  }

  function tryInsert() {
    if (document.querySelector('.kv-article-banner')) return true;
    const nav = document.querySelector('.kv-tool-nav');
    if (nav) { nav.insertAdjacentElement('afterend', buildBanner()); return true; }
    return false;
  }

  function init() {
    if (!tryInsert()) {
      const observer = new MutationObserver(function () {
        if (tryInsert()) observer.disconnect();
      });
      observer.observe(document.body || document.documentElement, { childList: true, subtree: true });
    }
  }

  if (document.body) { init(); } else { document.addEventListener('DOMContentLoaded', init); }
}());
```

- [ ] **Step 2: Commit**

```bash
cd "C:\Projects\StockAnalysis"
git add www/kv-banner.js
git commit -m "feat: add kv-banner.js for site-wide latest article banner"
```

---

## Task 3: Build the articles listing page

**Files:**
- Modify: `C:\Projects\Articles\www\index.html`

- [ ] **Step 1: Write the articles listing page**

Replace `C:\Projects\Articles\www\index.html` with:

```html
<!DOCTYPE html>
<html lang="en" class="dark">
<head>
  <script src="/kv-theme.js"></script>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="theme-color" content="#0f172a">
  <title>Articles — KashVector</title>
  <meta name="description" content="Plain-English guides to Australian tax, superannuation, property, and personal finance. Free calculators and articles — no sign-up required.">
  <link rel="stylesheet" href="/brand.css">
  <link rel="stylesheet" href="article.css">
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "WebPage",
    "name": "Articles — KashVector",
    "url": "https://kashvector.com/articles/",
    "description": "Plain-English guides to Australian tax, superannuation, property, and personal finance.",
    "publisher": { "@type": "Organization", "name": "KashVector", "url": "https://kashvector.com" }
  }
  </script>
</head>
<body>
  <header class="app-header">
    <a href="/" class="home-logo" aria-label="All KashVector tools">
      <img src="/logo.svg" alt="KashVector" width="32" height="32">
      <span class="home-logo-label">← All tools</span>
    </a>
    <div class="app-header-center">
      <h1>Articles</h1>
      <p>Plain-English guides to Australian personal finance</p>
    </div>
    <button class="theme-toggle" onclick="window.kvTheme.toggle()" aria-label="Toggle theme"></button>
  </header>

  <script src="/kv-nav.js"></script>
  <script src="/kv-banner.js"></script>

  <main>
    <div class="articles-list">
      <div class="article-entry">
        <div class="article-entry-date">19 May 2026</div>
        <div class="article-entry-title">
          <a href="/articles/2026-budget-what-changes/">2026 Federal Budget: What Changes for Everyday Australians</a>
        </div>
        <div class="article-entry-desc">Income tax cuts, negative gearing restrictions, and CGT changes — explained in plain English, with the numbers that matter to you.</div>
        <a class="article-entry-read" href="/articles/2026-budget-what-changes/">Read → (6 min)</a>
      </div>
    </div>
  </main>

  <footer>
    <a href="/">← All KashVector tools</a>
    &nbsp;·&nbsp;
    <a href="/privacy-site.html">Privacy</a>
  </footer>
</body>
</html>
```

- [ ] **Step 2: Verify locally**

With server running on port 8001 (`npx http-server www -p 8001 -c-1` from `C:\Projects\Articles`):

Open `http://localhost:8001/`. Expect:
- Dark background, "Articles" heading, one article entry visible
- The article entry link, date, description, and "Read →" all present
- No console errors (404s for `/kv-theme.js`, `/brand.css`, `/kv-nav.js`, `/kv-banner.js` are expected locally)

- [ ] **Step 3: Commit**

```bash
cd "C:\Projects\Articles"
git add www/index.html
git commit -m "feat: add articles listing page"
```

---

## Task 4: Build the first article page

**Files:**
- Modify: `C:\Projects\Articles\www\2026-budget-what-changes\index.html`

- [ ] **Step 1: Write the article page**

Replace `C:\Projects\Articles\www\2026-budget-what-changes\index.html` with the full article:

```html
<!DOCTYPE html>
<html lang="en" class="dark">
<head>
  <script src="/kv-theme.js"></script>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="theme-color" content="#0f172a">
  <title>2026 Federal Budget: What Changes for Everyday Australians — KashVector</title>
  <meta name="description" content="The 2026 budget cuts income tax, restricts negative gearing on established properties, and overhauls capital gains tax. Here's what it means for you — in plain English.">
  <link rel="stylesheet" href="/brand.css">
  <link rel="stylesheet" href="../article.css">
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "Article",
    "headline": "2026 Federal Budget: What Changes for Everyday Australians",
    "datePublished": "2026-05-19",
    "author": { "@type": "Organization", "name": "KashVector" },
    "publisher": { "@type": "Organization", "name": "KashVector", "url": "https://kashvector.com" },
    "url": "https://kashvector.com/articles/2026-budget-what-changes/",
    "description": "The 2026 budget cuts income tax, restricts negative gearing on established properties, and overhauls capital gains tax."
  }
  </script>
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "FAQPage",
    "mainEntity": [
      {
        "@type": "Question",
        "name": "When do the 2026 budget changes actually start?",
        "acceptedAnswer": { "@type": "Answer", "text": "The income tax cuts start 1 July 2026. The negative gearing restriction and CGT overhaul both take effect 1 July 2027 — but the cut-off date for which properties and assets are affected is 12 May 2026 (Budget night)." }
      },
      {
        "@type": "Question",
        "name": "Does the negative gearing change affect my existing investment property?",
        "acceptedAnswer": { "@type": "Answer", "text": "No. If you already own an investment property purchased before 12 May 2026, you are fully grandfathered. The restriction only applies to established properties purchased on or after 12 May 2026." }
      },
      {
        "@type": "Question",
        "name": "What is the CGT discount and why is it changing?",
        "acceptedAnswer": { "@type": "Answer", "text": "The 50% CGT discount meant you only paid tax on half of a capital gain if you held an asset for more than a year. The government is replacing this with inflation indexation (which adjusts your cost base for inflation) and a 30% minimum tax rate on the remaining gain. The change applies to assets acquired after 12 May 2026." }
      },
      {
        "@type": "Question",
        "name": "Is this article financial advice?",
        "acceptedAnswer": { "@type": "Answer", "text": "No. This article is general information only. Your personal circumstances — income, existing investments, state of residence — determine how these changes affect you. We recommend speaking with a registered financial adviser before making investment decisions." }
      }
    ]
  }
  </script>
</head>
<body>
  <header class="app-header">
    <a href="/" class="home-logo" aria-label="All KashVector tools">
      <img src="/logo.svg" alt="KashVector" width="32" height="32">
      <span class="home-logo-label">← All tools</span>
    </a>
    <div class="app-header-center">
      <h1>KashVector</h1>
      <p>Australian financial calculators</p>
    </div>
    <button class="theme-toggle" onclick="window.kvTheme.toggle()" aria-label="Toggle theme"></button>
  </header>

  <script src="/kv-nav.js"></script>
  <script src="/kv-banner.js"></script>

  <main>
    <article class="article-body">

      <div class="article-breadcrumb">
        <a href="/articles/">← Articles</a>
      </div>

      <h1 class="article-headline">2026 Federal Budget: What Changes for Everyday Australians</h1>
      <div class="article-byline">KashVector &nbsp;·&nbsp; 19 May 2026 &nbsp;·&nbsp; 6 min read</div>

      <p>Every May, the Treasurer delivers a Federal Budget. And every May, you wonder whether any of it actually affects you. This year, it does — in three ways that are worth understanding before you make any financial decisions.</p>

      <div class="article-summary">
        <p>Here's the short version:</p>
        <ul>
          <li>Your income tax bill is going down, starting 1 July 2026</li>
          <li>If you're thinking of buying an established investment property, the tax rules just changed significantly</li>
          <li>The way capital gains tax works on assets you buy from now on has been overhauled</li>
        </ul>
      </div>

      <h2>Your tax bill is going down</h2>

      <p>Good news first. From 1 July 2026, income tax rates are dropping across the board.</p>

      <p>The rate on income between $18,201 and $45,000 drops from 16% to 15%. On its own, that's worth about $268 extra in your pocket per year for an average earner on $81,000. From 1 July 2027, the rate drops again to 14% — worth up to $536 a year compared to where rates were in 2024–25.</p>

      <p>The upper thresholds are also shifting. The 30% bracket now runs all the way to $135,000 (previously $120,000), and the 37% bracket extends to $190,000 (previously $180,000). If you're earning in that $120k–$135k band, you've dropped from a 37% marginal rate to 30%. That's a meaningful difference — around $4,500 less tax per year at that income level.</p>

      <p>There's also a new $1,000 instant work expense deduction from the 2026–27 financial year. You can reduce your taxable income by $1,000 for work-related costs — phone, uniform, tools, home office — without keeping a single receipt. For most people, that's worth around $150–$320 at tax time depending on which bracket you're in.</p>

      <a class="article-cta" href="/budget/">
        <span class="article-cta-label">Try the free tool</span>
        <span class="article-cta-text">See your take-home pay under the new rates → Budget Planner</span>
      </a>

      <h2>Thinking of buying an investment property? The rules just changed</h2>

      <p>For a long time, owning a negatively geared property has been one of Australia's most popular tax strategies. Here's the idea: if your investment property costs you more than it earns in rent — say your mortgage interest and maintenance come to $3,500 a month but you only collect $3,000 in rent — that $500 monthly loss used to reduce your taxable salary income. At a 32% marginal rate, that's worth around $1,920 back from the ATO each year.</p>

      <p>From 1 July 2027, that changes — but only for established properties purchased on or after 12 May 2026 (Budget night).</p>

      <p>Under the new rules, rental losses can no longer be offset against your salary. Instead, they're quarantined. You carry them forward and use them against future rental income from the same property, or against the capital gain when you eventually sell. They're not lost — they just don't give you an immediate tax refund while you're holding the property.</p>

      <p><strong>Who isn't affected:</strong></p>
      <ul>
        <li>Existing investors who purchased before 12 May 2026 — you're fully grandfathered, nothing changes</li>
        <li>Buyers of brand new properties — new builds are exempt from the restriction and remain fully deductible</li>
      </ul>

      <p><strong>Who is affected:</strong></p>
      <ul>
        <li>Anyone purchasing an existing (established) investment property from 12 May 2026 onwards</li>
      </ul>

      <p>The practical impact: buying a negatively geared established property can still make sense in some scenarios, but the cash flow maths has changed. You're no longer receiving that annual tax top-up while you hold it. If you're weighing up a purchase right now, it's worth running the numbers with and without the deduction to understand what you're actually committing to.</p>

      <a class="article-cta" href="/budget-impact/">
        <span class="article-cta-label">Try the free tool</span>
        <span class="article-cta-text">Model the impact on your property → Budget Impact Calculator</span>
      </a>

      <h2>Capital gains tax is being overhauled</h2>

      <p>Capital gains tax (CGT) is what you pay when you sell an asset — shares, an investment property, cryptocurrency, or most other investments — for more than you paid for it. For the past 25 years, the deal for Australian investors has been simple: hold an asset for more than a year, and the taxable gain is halved. That's the 50% CGT discount.</p>

      <p>For assets acquired after 12 May 2026, that discount is gone.</p>

      <p>The replacement: your original purchase price gets indexed for inflation, and you pay tax on whatever gain remains — at a minimum rate of 30%.</p>

      <p>Here's a concrete example. You invest $100,000 today and sell in 10 years for $200,000 — a $100,000 nominal gain.</p>
      <ul>
        <li><strong>Old rules:</strong> you pay tax on $50,000 (half the gain). At a 32% marginal rate, that's $16,000 in CGT.</li>
        <li><strong>New rules:</strong> your $100,000 cost base is indexed for inflation — at 2.5% p.a. over 10 years, that's roughly $128,000. You pay 30% on the remaining $72,000 gain. That's $21,600 in CGT.</li>
      </ul>

      <p>For high-growth assets — property in sought-after areas, strong-performing shares — the old 50% discount was clearly better. But for lower-growth assets where most of the nominal gain was just inflation keeping pace, the new rules can actually produce a lower tax bill. The crossover depends on how much real growth your investment achieves above inflation.</p>

      <p><strong>What about assets you already own?</strong> Assets bought before 12 May 2026 keep the 50% discount — but only for gains accrued up to 1 July 2027. After that date, a split rule applies: gains from before 1 July 2027 use the old 50% discount, and gains from after that date use the new indexation approach. If you sell before 1 July 2027, the old rules apply in full.</p>

      <a class="article-cta" href="/budget-impact/">
        <span class="article-cta-label">Try the free tool</span>
        <span class="article-cta-text">Compare old vs new CGT rules on your investment → Budget Impact Calculator</span>
      </a>

      <h2>What to do now</h2>

      <p>These three changes don't all land at once — the tax cuts start 1 July 2026, while the investment property and CGT rules shift from 1 July 2027. But the date that determines which properties and assets are affected is already here. If you're planning a property purchase, weighing up an investment, or just curious what your take-home pay looks like under the new rates, the time to model it is now — before you commit, not after.</p>

      <a class="article-cta" href="/budget-impact/">
        <span class="article-cta-label">Free, no sign-up required</span>
        <span class="article-cta-text">See your personalised budget impact → Budget Impact Calculator</span>
      </a>

      <div class="article-disclaimer">
        This article is general information only and does not constitute financial advice. Tax rules are complex and your personal circumstances — income, existing investments, state of residence, and timing — will determine how these changes affect you. We recommend speaking with a registered financial adviser or tax professional before making investment decisions.
      </div>

    </article>

    <section class="tool-info">
      <div>
        <h2>Frequently asked questions</h2>
        <details class="faq-item">
          <summary>When do these changes actually start?</summary>
          <p>The income tax cuts start 1 July 2026. The negative gearing restriction and CGT overhaul both take effect 1 July 2027 — but the cut-off date for which properties and assets are affected is 12 May 2026 (Budget night). Anything purchased on or after that date falls under the new rules from 1 July 2027.</p>
        </details>
        <details class="faq-item">
          <summary>Does the negative gearing change affect my existing investment property?</summary>
          <p>No. If you purchased your investment property before 12 May 2026, you are fully grandfathered. The restriction only applies to established properties purchased on or after 12 May 2026. New builds purchased after that date are also exempt.</p>
        </details>
        <details class="faq-item">
          <summary>What is the CGT discount and why is it changing?</summary>
          <p>The 50% CGT discount meant you only paid tax on half your capital gain if you held an asset for more than a year. The government is replacing this with inflation indexation — your cost base is adjusted for CPI — plus a 30% minimum tax rate on the remaining gain. The change applies to assets acquired after 12 May 2026.</p>
        </details>
        <details class="faq-item">
          <summary>Is this article financial advice?</summary>
          <p>No. This article is general information only. Your personal circumstances — income, existing investments, state of residence — determine how these changes affect you. We recommend speaking with a registered financial adviser before making investment decisions.</p>
        </details>
      </div>
    </section>
  </main>

  <footer>
    <a href="/">← All KashVector tools</a>
    &nbsp;·&nbsp;
    <a href="/articles/">Articles</a>
    &nbsp;·&nbsp;
    <a href="/privacy-site.html">Privacy</a>
  </footer>
</body>
</html>
```

- [ ] **Step 2: Verify article locally**

With local server running on port 8001:

Open `http://localhost:8001/2026-budget-what-changes/`. Verify:
- Headline, byline, breadcrumb all render
- Three sections visible with correct headings
- Three CTA cards appear with sky border
- FAQ section opens/closes correctly
- Disclaimer renders at the bottom

- [ ] **Step 3: Commit**

```bash
cd "C:\Projects\Articles"
git add "www/2026-budget-what-changes/index.html"
git commit -m "feat: add 2026 Federal Budget article"
```

---

## Task 5: Update the hub landing page

**Files:**
- Modify: `C:\Projects\StockAnalysis\www\index.html`

- [ ] **Step 1: Add the kv-banner.js script tag to the hub**

In `C:\Projects\StockAnalysis\www\index.html`, find the existing `<script src="/kv-nav.js"></script>` line and add the banner script immediately after it:

```html
<script src="/kv-nav.js"></script>
<script src="/kv-banner.js"></script>
```

- [ ] **Step 2: Add the "From the blog" section**

In `C:\Projects\StockAnalysis\www\index.html`, find the `<footer>` tag and insert the following block immediately before it:

```html
<!-- From the blog -->
<section id="from-the-blog" style="max-width:900px;margin:48px auto 16px;padding:0 20px;">
  <h2 style="font-size:1rem;font-weight:600;color:#f1f5f9;margin-bottom:16px;padding-bottom:8px;border-bottom:1px solid #334155;">From the blog</h2>
  <div style="display:grid;grid-template-columns:repeat(auto-fill,minmax(280px,1fr));gap:16px;">
    <a href="/articles/2026-budget-what-changes/"
       class="blog-card"
       style="display:block;background:#1e293b;border:1px solid #334155;border-radius:10px;padding:20px;text-decoration:none;transition:border-color 0.15s;">
      <div style="font-size:0.75rem;color:#94a3b8;margin-bottom:6px;">19 May 2026</div>
      <div style="font-size:1rem;font-weight:600;color:#f1f5f9;margin-bottom:8px;line-height:1.4;">2026 Federal Budget: What Changes for Everyday Australians</div>
      <div style="font-size:0.85rem;color:#94a3b8;line-height:1.5;margin-bottom:12px;">Income tax cuts, negative gearing restrictions, and CGT changes — explained in plain English.</div>
      <span style="font-size:0.82rem;color:#38bdf8;font-weight:500;">Read → (6 min)</span>
    </a>
  </div>
</section>
```

- [ ] **Step 3: Add light mode CSS for the blog section**

In `C:\Projects\StockAnalysis\www\index.html`, find the existing `<style>` block (it already contains inline CSS for the hub). Add these rules inside it:

```css
html:not(.dark) #from-the-blog h2 { color: #0f172a; border-color: #e2e8f0; }
html:not(.dark) .blog-card { background: #d4dce8 !important; border-color: #e2e8f0 !important; }
html:not(.dark) .blog-card div[style*="color:#f1f5f9"] { color: #0f172a !important; }
html:not(.dark) .blog-card div[style*="color:#94a3b8"] { color: #64748b !important; }
html:not(.dark) .blog-card:hover { border-color: #0ea5e9 !important; }
```

- [ ] **Step 4: Verify locally**

Serve StockAnalysis locally:
```bash
cd "C:\Projects\StockAnalysis"
npx --yes http-server www -p 8000 -c-1
```

Open `http://localhost:8000`. Verify:
- "From the blog" section appears below the tool grid with the article card
- Toggle to light mode — blog card background and text adjust correctly
- Banner appears below the cross-tool nav (the banner loads from `/kv-banner.js` which now exists)

- [ ] **Step 5: Commit**

```bash
cd "C:\Projects\StockAnalysis"
git add www/index.html
git commit -m "feat: add kv-banner and From the blog section to hub"
```

---

## Task 6: Add banner to all vanilla tool pages

**Files:**
- Modify: `www\stock\index.html`, `www\fire\index.html`, `www\mortgage\index.html`, `www\debt-recycling\index.html`, `www\portfolio-health\index.html`, `www\super-compare\index.html`, `www\life-buyback\index.html`, `www\rent-vs-buy\index.html`, `www\budget-impact\index.html`

All 9 files need the same change: add `<script src="/kv-banner.js"></script>` immediately after the existing `<script src="/kv-nav.js"></script>` line.

- [ ] **Step 1: Add banner script to all 9 tool pages**

For each of these files, find `<script src="/kv-nav.js"></script>` and add the banner line after it:

```
C:\Projects\StockAnalysis\www\stock\index.html
C:\Projects\StockAnalysis\www\fire\index.html
C:\Projects\StockAnalysis\www\mortgage\index.html
C:\Projects\StockAnalysis\www\debt-recycling\index.html
C:\Projects\StockAnalysis\www\portfolio-health\index.html
C:\Projects\StockAnalysis\www\super-compare\index.html
C:\Projects\StockAnalysis\www\life-buyback\index.html
C:\Projects\StockAnalysis\www\rent-vs-buy\index.html
C:\Projects\StockAnalysis\www\budget-impact\index.html
```

In each file, replace:
```html
<script src="/kv-nav.js"></script>
```
with:
```html
<script src="/kv-nav.js"></script>
<script src="/kv-banner.js"></script>
```

- [ ] **Step 2: Verify one tool page locally**

Open `http://localhost:8000/fire/`. Verify:
- Banner strip appears below the cross-tool nav
- Dark mode: dark card background, sky-blue left border, white text
- Light mode (toggle): lighter background, blue border, dark text
- No console errors

- [ ] **Step 3: Commit**

```bash
cd "C:\Projects\StockAnalysis"
git add www/stock/index.html www/fire/index.html www/mortgage/index.html \
        www/debt-recycling/index.html www/portfolio-health/index.html \
        www/super-compare/index.html www/life-buyback/index.html \
        www/rent-vs-buy/index.html www/budget-impact/index.html
git commit -m "feat: add kv-banner to all vanilla tool pages"
```

---

## Task 7: Add banner to Budget Planner

**Files:**
- Create: `C:\Projects\Budget Planner\public\kv-banner.js`
- Modify: `C:\Projects\Budget Planner\index.html`

- [ ] **Step 1: Copy kv-banner.js to Budget Planner public folder**

```bash
cp "C:\Projects\StockAnalysis\www\kv-banner.js" "C:\Projects\Budget Planner\public\kv-banner.js"
```

- [ ] **Step 2: Add script tag to Budget Planner index.html**

In `C:\Projects\Budget Planner\index.html`, find the existing `<script src="/kv-nav.js"></script>` line and add the banner script immediately after it:

```html
<script src="/kv-nav.js"></script>
<script src="/kv-banner.js"></script>
```

- [ ] **Step 3: Start Budget Planner dev server and verify**

```bash
cd "C:\Projects\Budget Planner"
npm run dev
```

Open `http://localhost:5173`. Verify:
- Banner appears below the cross-tool nav on the wizard and dashboard
- MutationObserver correctly picks up the nav after React renders
- Banner text links to `/articles/2026-budget-what-changes/`
- No console errors

- [ ] **Step 4: Commit**

```bash
cd "C:\Projects\Budget Planner"
git add public/kv-banner.js index.html
git commit -m "feat: add kv-banner to Budget Planner"
```

---

## Task 8: Deploy

- [ ] **Step 1: Deploy the Articles pages to StockAnalysis**

```bash
cp -r "C:/Projects/Articles/www/." "C:/Projects/StockAnalysis/www/articles/"
```

- [ ] **Step 2: Verify the copy landed correctly**

```bash
ls "C:\Projects\StockAnalysis\www\articles"
# Expected: index.html  article.css  2026-budget-what-changes/
ls "C:\Projects\StockAnalysis\www\articles\2026-budget-what-changes"
# Expected: index.html
```

- [ ] **Step 3: Add articles to sitemap**

In `C:\Projects\StockAnalysis\www\sitemap.xml`, add two new `<url>` blocks. Find the last `</url>` closing tag and add after it:

```xml
<url>
  <loc>https://kashvector.com/articles/</loc>
  <changefreq>weekly</changefreq>
  <priority>0.7</priority>
</url>
<url>
  <loc>https://kashvector.com/articles/2026-budget-what-changes/</loc>
  <changefreq>monthly</changefreq>
  <priority>0.8</priority>
</url>
```

- [ ] **Step 4: Commit and push StockAnalysis**

```bash
cd "C:\Projects\StockAnalysis"
git add www/articles/ www/sitemap.xml
git commit -m "feat: deploy articles section and 2026 budget article"
git push
```

- [ ] **Step 5: Build and deploy Budget Planner**

```bash
cd "C:\Projects\Budget Planner"
MSYS_NO_PATHCONV=1 VITE_BASE_PATH='/budget/' npm run build
cp -r "C:/Projects/Budget Planner/dist/." "C:/Projects/StockAnalysis/www/budget/"
cd "C:\Projects\StockAnalysis"
git add www/budget/
git commit -m "deploy: Budget Planner with kv-banner"
git push
```

- [ ] **Step 6: Verify on the live site**

Wait ~1 minute for Cloudflare Pages to deploy. Then verify:

| Check | URL | What to look for |
|---|---|---|
| Articles index | `kashvector.com/articles/` | Listing page renders, article card visible |
| Article page | `kashvector.com/articles/2026-budget-what-changes/` | Full article, 3 CTAs, FAQ, disclaimer |
| Banner on hub | `kashvector.com/` | Slim banner below cross-tool nav |
| Banner on tool | `kashvector.com/fire/` | Banner appears correctly |
| Banner on Budget Planner | `kashvector.com/budget/` | Banner appears via MutationObserver |
| Light mode | Any page | Banner and blog card respond to theme toggle |
| Article CTAs | Click "Budget Planner" CTA | Navigates to `/budget/` |
| Article CTAs | Click "Budget Impact" CTAs | Navigates to `/budget-impact/` |
| Mobile | Article page at 390px | Headline readable, CTAs not truncated, FAQ works |

- [ ] **Step 7: Submit to Google Search Console**

If `kashvector.com` is in Google Search Console, submit the new URLs for indexing:
- `https://kashvector.com/articles/`
- `https://kashvector.com/articles/2026-budget-what-changes/`

---

## Updating the banner for future articles

When publishing a new article, the only file to update in StockAnalysis is `www/kv-banner.js`:

```js
const LATEST = {
  title: 'Your New Article Title Here',
  href: '/articles/your-new-article-slug/'
};
```

Then copy to `C:\Projects\Budget Planner\public\kv-banner.js`, rebuild Budget Planner, and redeploy both repos.
