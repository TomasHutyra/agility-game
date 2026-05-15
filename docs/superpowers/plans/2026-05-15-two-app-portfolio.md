# Two-App Portfolio Website Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Restructure www.bookdragondev.com from a single-product Agility Game site to a two-product Bookdragon Development portfolio featuring Agility Game and QuestDeck.

**Architecture:** Option C — symmetric portfolio. `index.html` becomes a company homepage with two product cards. Each app gets its own dedicated landing page (`agility-game.html`, `questdeck.html`). Each app has its own privacy policy page.

**Tech Stack:** Plain HTML5, CSS3 (custom properties), vanilla JS (inline). No build step. Deployed on Netlify.

---

## File Map

| Action | File | What changes |
|---|---|---|
| Create | `.gitignore` | Exclude `.superpowers/` |
| Copy | `images/questdeck-icon.png` | QuestDeck app icon from assets |
| Modify | `style.css` | New CSS variables and components |
| Rewrite | `index.html` | Bookdragon homepage with two product cards |
| Create | `agility-game.html` | Current `index.html` content, nav updated, canonical URLs fixed |
| Modify | `privacy-policy.html` | Nav updated only |
| Modify | `contact.html` | Nav updated only |
| Create | `questdeck.html` | QuestDeck landing page |
| Create | `questdeck-privacy-policy.html` | QuestDeck privacy policy (exact text from MD) |
| Modify | `netlify.toml` | Three new 301 redirects |
| Modify | `sitemap.xml` | Updated domain + new URLs |

**Serve locally for verification:** `npx serve .` then open `http://localhost:3000`

---

## Task 1: Housekeeping — .gitignore and icon asset

**Files:**
- Create: `.gitignore`
- Copy: `images/questdeck-icon.png`

- [ ] **Step 1: Create .gitignore**

```
.superpowers/
```

Save to `.gitignore` in project root.

- [ ] **Step 2: Copy QuestDeck icon**

```powershell
Copy-Item "C:\Projects\AI\Claude\QuestDeck\assets\QD_ikon.png" "c:\Projects\Web\www.bookdragondev.com\images\questdeck-icon.png"
```

- [ ] **Step 3: Verify icon exists**

```powershell
Test-Path "c:\Projects\Web\www.bookdragondev.com\images\questdeck-icon.png"
```
Expected: `True`

- [ ] **Step 4: Commit**

```bash
git add .gitignore images/questdeck-icon.png
git commit -m "chore: add .gitignore and QuestDeck icon asset"
```

---

## Task 2: CSS — new variables and components

**Files:**
- Modify: `style.css`

Append the following block at the end of `style.css`, just before the closing of the last `@media` block (i.e. before the final `}`). Actually append after all existing rules as a new section.

- [ ] **Step 1: Add QuestDeck accent variable to `:root`**

In `style.css`, inside `:root { }` (lines 3–17), add after `--btn-text: #393939;`:

```css
  --accent-qd:  #FF8C42;
  --accent-qd-h: #e07a32;
```

- [ ] **Step 2: Add new component styles**

Append to the end of `style.css`:

```css
/* ===== QD BUTTON ===== */
.btn-qd {
  display: inline-block;
  padding: 10px 24px;
  background: var(--accent-qd);
  color: #fff;
  font-size: 0.88rem;
  font-weight: 700;
  border: 2px solid var(--accent-qd);
  border-radius: 4px;
  letter-spacing: .03em;
  transition: background .15s, border-color .15s;
}
.btn-qd:hover {
  background: var(--accent-qd-h);
  border-color: var(--accent-qd-h);
}

/* ===== BADGE ===== */
.badge {
  display: inline-block;
  background: var(--bg-dark2);
  color: var(--muted);
  border: 1px solid var(--border);
  border-radius: 3px;
  padding: 3px 8px;
  font-size: 0.75rem;
}
.badge--alpha {
  background: var(--accent-qd);
  color: #fff;
  border-color: var(--accent-qd);
}

/* ===== PRODUCT CARDS (homepage) ===== */
.product-cards {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 16px;
  max-width: var(--max-w);
  margin: 48px auto;
  padding: 0 24px;
}

.product-card {
  background: var(--bg-alt);
  border: 1px solid var(--border);
  border-radius: 8px;
  overflow: hidden;
}

.product-card__accent { height: 6px; }

.product-card__body { padding: 28px 24px; }

.product-card__header {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 16px;
}

.product-card__icon {
  width: 48px;
  height: 48px;
  border-radius: 8px;
  object-fit: contain;
  background: var(--bg-dark2);
  border: 1px solid var(--border);
  flex-shrink: 0;
}

.product-card__title {
  color: var(--heading);
  font-weight: 700;
  font-size: 1.05rem;
}

.product-card__platform {
  color: var(--muted);
  font-size: 0.78rem;
  margin-top: 2px;
}

.product-card__desc {
  color: var(--text);
  font-size: 0.88rem;
  line-height: 1.6;
  margin-bottom: 20px;
}

.product-card__badges {
  display: flex;
  gap: 6px;
  flex-wrap: wrap;
  margin-bottom: 20px;
}

/* ===== HOME HERO ===== */
.home-hero {
  padding: 60px 24px 48px;
  text-align: center;
  border-bottom: 1px solid var(--border);
}

.home-hero__label {
  color: var(--accent);
  font-size: 0.8rem;
  letter-spacing: .12em;
  text-transform: uppercase;
  margin-bottom: 12px;
}

.home-hero__title {
  color: var(--heading);
  font-size: clamp(1.6rem, 3vw, 2.2rem);
  font-weight: 700;
  line-height: 1.2;
  margin-bottom: 12px;
}

.home-hero__sub {
  color: var(--muted);
  font-size: 0.95rem;
  max-width: 480px;
  margin: 0 auto;
}

/* ===== QUESTDECK PAGE ===== */
.qd-hero { border-bottom: 1px solid var(--border); }

.qd-hero__inner {
  max-width: var(--max-w);
  margin: 0 auto;
  padding: 60px 24px;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 48px;
  align-items: center;
}

.qd-hero__icon-wrap {
  background: var(--bg-alt);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.qd-hero__icon {
  width: 120px;
  height: 120px;
  border-radius: 24px;
  object-fit: contain;
}

.qd-steps {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 16px;
  max-width: var(--max-w);
  margin: 0 auto;
  padding: 48px 24px;
}

.qd-step {
  background: var(--bg-alt);
  border-radius: 6px;
  padding: 20px;
  border-left: 3px solid var(--accent-qd);
}

.qd-step__label {
  color: var(--accent-qd);
  font-weight: 700;
  font-size: 0.85rem;
  margin-bottom: 8px;
}

.qd-step__text {
  color: var(--text);
  font-size: 0.85rem;
  line-height: 1.5;
}

.qd-features {
  max-width: var(--max-w);
  margin: 0 auto;
  padding: 48px 24px;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 10px;
}

.qd-feature {
  display: flex;
  gap: 8px;
  align-items: flex-start;
  color: var(--text);
  font-size: 0.88rem;
}

.qd-feature__check {
  color: var(--accent-qd);
  flex-shrink: 0;
  font-weight: 700;
}

.qd-cta {
  text-align: center;
  padding: 60px 24px;
  background: var(--bg-alt);
}

.qd-cta__label {
  color: var(--accent-qd);
  font-size: 0.78rem;
  letter-spacing: .1em;
  text-transform: uppercase;
  margin-bottom: 12px;
}

.qd-cta__title {
  color: var(--heading);
  font-size: clamp(1.3rem, 2.5vw, 1.8rem);
  font-weight: 700;
  margin-bottom: 12px;
}

.qd-cta__text {
  color: var(--muted);
  font-size: 0.9rem;
  margin-bottom: 24px;
}

/* ===== HIGHLIGHT BOX (QD privacy policy) ===== */
.highlight-box {
  background: var(--bg-alt);
  border-left: 4px solid var(--accent-qd);
  border-radius: 4px;
  padding: 16px 20px;
  margin-bottom: 28px;
}

.highlight-box__title {
  color: var(--heading);
  font-weight: 700;
  margin-bottom: 4px;
}

.highlight-box__sub {
  color: var(--muted);
  font-size: 0.85rem;
}

/* QuestDeck page-content link color override */
.page-content--qd a { color: var(--accent-qd); }
.page-content--qd a:hover { text-decoration: underline; }

/* ===== RESPONSIVE ADDITIONS ===== */
@media (max-width: 768px) {
  .product-cards { grid-template-columns: 1fr; }
  .qd-hero__inner { grid-template-columns: 1fr; gap: 32px; padding: 40px 16px; }
  .qd-steps { grid-template-columns: 1fr; padding: 32px 16px; }
  .qd-features { grid-template-columns: 1fr; padding: 32px 16px; }
}
```

- [ ] **Step 3: Verify CSS parses without errors**

Open browser dev tools console at `http://localhost:3000` — no CSS errors.

- [ ] **Step 4: Commit**

```bash
git add style.css
git commit -m "feat: add QuestDeck CSS variables and portfolio components"
```

---

## Task 3: Create agility-game.html

**Files:**
- Create: `agility-game.html` (copy of current `index.html`, three changes: nav, footer, canonical URLs)

This file is the current `index.html` with:
1. Nav updated to new structure
2. Canonical/OG/Twitter URLs changed from `agility-game.vercel.app` to `www.bookdragondev.com/agility-game.html`
3. Schema.org URL updated

- [ ] **Step 1: Copy index.html to agility-game.html**

```powershell
Copy-Item "c:\Projects\Web\www.bookdragondev.com\index.html" "c:\Projects\Web\www.bookdragondev.com\agility-game.html"
```

- [ ] **Step 2: Update `<head>` meta tags in agility-game.html**

Replace the four canonical/OG URL lines:

Old:
```html
  <link rel="canonical" href="https://agility-game.vercel.app/" />
```
New:
```html
  <link rel="canonical" href="https://www.bookdragondev.com/agility-game.html" />
```

Old:
```html
  <meta property="og:url" content="https://agility-game.vercel.app/" />
```
New:
```html
  <meta property="og:url" content="https://www.bookdragondev.com/agility-game.html" />
```

Old:
```html
  <meta property="og:image" content="https://agility-game.vercel.app/images/race.webp" />
```
New:
```html
  <meta property="og:image" content="https://www.bookdragondev.com/images/race.webp" />
```

Old:
```html
  <meta name="twitter:image" content="https://agility-game.vercel.app/images/race.webp" />
```
New:
```html
  <meta name="twitter:image" content="https://www.bookdragondev.com/images/race.webp" />
```

- [ ] **Step 3: Update Schema.org URLs in agility-game.html**

Replace all occurrences of `agility-game.vercel.app` in the JSON-LD block with `www.bookdragondev.com`.

Old:
```json
    "url": "https://agility-game.vercel.app/",
    "image": "https://agility-game.vercel.app/images/race.webp",
```
New:
```json
    "url": "https://www.bookdragondev.com/agility-game.html",
    "image": "https://www.bookdragondev.com/images/race.webp",
```

Old (publisher url):
```json
      "url": "https://agility-game.vercel.app/"
```
New:
```json
      "url": "https://www.bookdragondev.com/"
```

- [ ] **Step 4: Update nav in agility-game.html**

Replace the entire `<nav>` block:

Old:
```html
    <nav>
      <ul id="nav-menu">
        <li><a href="index.html" class="active">Agility Game</a></li>
        <li><a href="privacy-policy.html">Privacy policy</a></li>
        <li><a href="contact.html">Contact</a></li>
      </ul>
    </nav>
```
New:
```html
    <nav>
      <ul id="nav-menu">
        <li><a href="agility-game.html" class="active">Agility Game</a></li>
        <li><a href="questdeck.html">QuestDeck</a></li>
        <li><a href="contact.html">Contact</a></li>
      </ul>
    </nav>
```

- [ ] **Step 5: Verify in browser**

Open `http://localhost:3000/agility-game.html`. Check:
- Nav shows "Agility Game" (active/highlighted), "QuestDeck", "Contact"
- All existing sections render correctly
- Logo links back to `index.html`

- [ ] **Step 6: Commit**

```bash
git add agility-game.html
git commit -m "feat: add agility-game.html with updated nav and canonical URLs"
```

---

## Task 4: Rewrite index.html as Bookdragon homepage

**Files:**
- Modify: `index.html` (full rewrite)

- [ ] **Step 1: Replace index.html with new homepage**

Full content of `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <title>Bookdragon Development – Games &amp; Apps</title>
  <meta name="description" content="Bookdragon Development makes games and apps worth your time. Creators of Agility Game and QuestDeck." />
  <link rel="canonical" href="https://www.bookdragondev.com/" />

  <!-- Open Graph -->
  <meta property="og:type" content="website" />
  <meta property="og:url" content="https://www.bookdragondev.com/" />
  <meta property="og:title" content="Bookdragon Development – Games &amp; Apps" />
  <meta property="og:description" content="Bookdragon Development makes games and apps worth your time." />
  <meta property="og:site_name" content="Bookdragon Development" />

  <!-- Schema.org -->
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "Organization",
    "name": "Bookdragon Development",
    "url": "https://www.bookdragondev.com/",
    "email": "office@bookdragondev.com",
    "description": "Independent game and app studio. Creators of Agility Game and QuestDeck."
  }
  </script>

  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap" rel="stylesheet" />
  <link rel="icon" href="images/logo.png" type="image/png" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>

<header>
  <div class="nav-inner">
    <a href="index.html" class="logo">
      <img src="images/logo.png" alt="Bookdragon Development logo" width="40" height="40" />
      Bookdragon Development
    </a>
    <nav>
      <ul id="nav-menu">
        <li><a href="agility-game.html">Agility Game</a></li>
        <li><a href="questdeck.html">QuestDeck</a></li>
        <li><a href="contact.html">Contact</a></li>
      </ul>
    </nav>
    <button class="hamburger" id="hamburger" aria-label="Menu" aria-expanded="false">
      <span></span><span></span><span></span>
    </button>
  </div>
</header>

<div class="home-hero sc-d">
  <p class="home-hero__label">Bookdragon Development</p>
  <h1 class="home-hero__title">We make games and apps<br>worth your time.</h1>
  <p class="home-hero__sub">From competitive dog agility to real-life adventure cards — built with care, without the fluff.</p>
</div>

<section class="sc-d" aria-label="Our apps">
  <div class="product-cards">

    <div class="product-card">
      <div class="product-card__accent" style="background:var(--accent)"></div>
      <div class="product-card__body">
        <div class="product-card__header">
          <img class="product-card__icon" src="images/logo.png" alt="Agility Game icon" width="48" height="48" />
          <div>
            <div class="product-card__title">Agility Game</div>
            <div class="product-card__platform">PC · Android</div>
          </div>
        </div>
        <p class="product-card__desc">Build original courses, race your dog through jumps, tunnels and slaloms, and compete with players worldwide.</p>
        <div class="product-card__badges">
          <span class="badge">Epic Store</span>
          <span class="badge">Steam</span>
          <span class="badge">Google Play</span>
        </div>
        <a class="btn-store" href="agility-game.html">More info →</a>
      </div>
    </div>

    <div class="product-card">
      <div class="product-card__accent" style="background:var(--accent-qd)"></div>
      <div class="product-card__body">
        <div class="product-card__header">
          <img class="product-card__icon" src="images/questdeck-icon.png" alt="QuestDeck icon" width="48" height="48" />
          <div>
            <div class="product-card__title">QuestDeck</div>
            <div class="product-card__platform">Android · <span style="color:var(--accent-qd)">Alpha</span></div>
          </div>
        </div>
        <p class="product-card__desc">A pocket deck of real-world activity cards. Pick a mood, flip three cards, and go do something real. Offline. No account.</p>
        <div class="product-card__badges">
          <span class="badge">Google Play</span>
          <span class="badge badge--alpha">Coming soon</span>
        </div>
        <a class="btn-qd" href="questdeck.html">Join alpha →</a>
      </div>
    </div>

  </div>
</section>

<footer>
  <span>©&nbsp;2025&nbsp;Všechna práva vyhrazena</span>
  <a href="contact.html">Contact</a>
</footer>

<script>
  const hamburger = document.getElementById('hamburger');
  const menu = document.getElementById('nav-menu');
  hamburger.addEventListener('click', () => {
    const open = menu.classList.toggle('open');
    hamburger.setAttribute('aria-expanded', String(open));
  });
  menu.querySelectorAll('a').forEach(a =>
    a.addEventListener('click', () => menu.classList.remove('open'))
  );
</script>
</body>
</html>
```

- [ ] **Step 2: Verify in browser**

Open `http://localhost:3000`. Check:
- Hero text "We make games and apps worth your time." visible
- Two product cards side by side (desktop) / stacked (mobile)
- Agility Game card: Bookdragon logo icon, orange `#e08053` accent bar, "More info →" button
- QuestDeck card: QuestDeck icon, orange `#FF8C42` accent bar, "Alpha" badge, "Join alpha →" button
- Clicking "More info →" opens `agility-game.html`
- Clicking "Join alpha →" opens `questdeck.html` (404 for now, that's ok)
- Mobile: resize to <768px, cards stack to 1 column

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: rewrite index.html as Bookdragon portfolio homepage"
```

---

## Task 5: Update nav in privacy-policy.html and contact.html

**Files:**
- Modify: `privacy-policy.html`
- Modify: `contact.html`

- [ ] **Step 1: Update nav in privacy-policy.html**

Replace the `<nav>` block:

Old:
```html
    <nav>
      <ul id="nav-menu">
        <li><a href="index.html">Agility Game</a></li>
        <li><a href="privacy-policy.html" class="active">Privacy policy</a></li>
        <li><a href="contact.html">Contact</a></li>
      </ul>
    </nav>
```
New:
```html
    <nav>
      <ul id="nav-menu">
        <li><a href="agility-game.html">Agility Game</a></li>
        <li><a href="questdeck.html">QuestDeck</a></li>
        <li><a href="contact.html">Contact</a></li>
      </ul>
    </nav>
```

Also update the canonical/OG URLs in `privacy-policy.html` `<head>`:

Old:
```html
  <link rel="canonical" href="https://agility-game.vercel.app/privacy-policy.html" />
  <meta property="og:url" content="https://agility-game.vercel.app/privacy-policy.html" />
```
New:
```html
  <link rel="canonical" href="https://www.bookdragondev.com/privacy-policy.html" />
  <meta property="og:url" content="https://www.bookdragondev.com/privacy-policy.html" />
```

- [ ] **Step 2: Update nav in contact.html**

Replace the `<nav>` block:

Old:
```html
    <nav>
      <ul id="nav-menu">
        <li><a href="index.html">Agility Game</a></li>
        <li><a href="privacy-policy.html">Privacy policy</a></li>
        <li><a href="contact.html" class="active">Contact</a></li>
      </ul>
    </nav>
```
New:
```html
    <nav>
      <ul id="nav-menu">
        <li><a href="agility-game.html">Agility Game</a></li>
        <li><a href="questdeck.html">QuestDeck</a></li>
        <li><a href="contact.html" class="active">Contact</a></li>
      </ul>
    </nav>
```

Also update canonical/OG URL in `contact.html`:

Old:
```html
  <link rel="canonical" href="https://agility-game.vercel.app/contact.html" />
  <meta property="og:url" content="https://agility-game.vercel.app/contact.html" />
```
New:
```html
  <link rel="canonical" href="https://www.bookdragondev.com/contact.html" />
  <meta property="og:url" content="https://www.bookdragondev.com/contact.html" />
```

- [ ] **Step 3: Verify in browser**

Open `http://localhost:3000/privacy-policy.html` and `http://localhost:3000/contact.html`. Check:
- Nav shows "Agility Game", "QuestDeck", "Contact" (Contact active on contact.html)
- No "Privacy policy" in nav

- [ ] **Step 4: Commit**

```bash
git add privacy-policy.html contact.html
git commit -m "feat: update nav on privacy-policy and contact pages"
```

---

## Task 6: Create questdeck.html

**Files:**
- Create: `questdeck.html`

- [ ] **Step 1: Create questdeck.html**

Full content:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <title>QuestDeck – Real-Life Activity Cards | Bookdragon Development</title>
  <meta name="description" content="Flip a card. Do something real. QuestDeck is a pocket deck of 100+ real-world activity cards. Works offline, no account needed. Alpha testing on Android." />
  <link rel="canonical" href="https://www.bookdragondev.com/questdeck.html" />

  <!-- Open Graph -->
  <meta property="og:type" content="website" />
  <meta property="og:url" content="https://www.bookdragondev.com/questdeck.html" />
  <meta property="og:title" content="QuestDeck – Real-Life Activity Cards" />
  <meta property="og:description" content="Flip a card. Do something real. Works offline, no login needed." />
  <meta property="og:image" content="https://www.bookdragondev.com/images/questdeck-icon.png" />
  <meta property="og:site_name" content="Bookdragon Development" />

  <!-- Twitter Card -->
  <meta name="twitter:card" content="summary" />
  <meta name="twitter:title" content="QuestDeck – Real-Life Activity Cards" />
  <meta name="twitter:description" content="Flip a card. Do something real. Works offline, no login needed." />
  <meta name="twitter:image" content="https://www.bookdragondev.com/images/questdeck-icon.png" />

  <!-- Schema.org -->
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "MobileApplication",
    "name": "QuestDeck",
    "description": "A pocket deck of real-world activity cards. Pick a mood, flip three cards, and go do something real. Offline. No account.",
    "url": "https://www.bookdragondev.com/questdeck.html",
    "image": "https://www.bookdragondev.com/images/questdeck-icon.png",
    "operatingSystem": "Android",
    "applicationCategory": "LifestyleApplication",
    "publisher": {
      "@type": "Organization",
      "name": "Bookdragon Development",
      "email": "office@bookdragondev.com",
      "url": "https://www.bookdragondev.com/"
    }
  }
  </script>

  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap" rel="stylesheet" />
  <link rel="icon" href="images/logo.png" type="image/png" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>

<header>
  <div class="nav-inner">
    <a href="index.html" class="logo">
      <img src="images/logo.png" alt="Bookdragon Development logo" width="40" height="40" />
      Bookdragon Development
    </a>
    <nav>
      <ul id="nav-menu">
        <li><a href="agility-game.html">Agility Game</a></li>
        <li><a href="questdeck.html" class="active">QuestDeck</a></li>
        <li><a href="contact.html">Contact</a></li>
      </ul>
    </nav>
    <button class="hamburger" id="hamburger" aria-label="Menu" aria-expanded="false">
      <span></span><span></span><span></span>
    </button>
  </div>
</header>

<!-- ===== HERO ===== -->
<section class="qd-hero sc-d">
  <div class="qd-hero__inner">
    <div>
      <span class="badge badge--alpha" style="margin-bottom:16px">Alpha</span>
      <h1 style="color:var(--heading);font-size:clamp(1.6rem,2.5vw,2.2rem);font-weight:700;line-height:1.2;margin-bottom:12px;margin-top:12px">
        QuestDeck<br><span style="color:var(--accent-qd)">Real-Life Activity Cards</span>
      </h1>
      <p class="sub" style="margin-bottom:8px">Flip a card. Do something real. Works offline, no login needed.</p>
      <p class="sub" style="margin-bottom:24px">A pocket deck of 100+ real-world activity cards for bored moments, weekends, and friends.</p>
      <a class="btn-qd" href="mailto:office@bookdragondev.com">Join alpha testing →</a>
      <p style="margin-top:12px;font-size:0.82rem;color:var(--muted)">
        Email us: <a href="mailto:office@bookdragondev.com" style="color:var(--accent-qd)">office@bookdragondev.com</a>
      </p>
    </div>
    <div class="qd-hero__icon-wrap">
      <img class="qd-hero__icon" src="images/questdeck-icon.png" alt="QuestDeck app icon" width="120" height="120" />
    </div>
  </div>
</section>

<!-- ===== HOW IT WORKS ===== -->
<section class="sc-cd" aria-label="How QuestDeck works">
  <div style="max-width:var(--max-w);margin:0 auto;padding:48px 24px 24px">
    <h2 style="color:var(--heading);font-size:1.2rem;font-weight:700">How it works</h2>
  </div>
  <div class="qd-steps" style="padding-top:0">
    <div class="qd-step">
      <div class="qd-step__label">1. Pick a mood</div>
      <p class="qd-step__text">Bored, outside, with friends, weekend — 8 moods to choose from.</p>
    </div>
    <div class="qd-step">
      <div class="qd-step__label">2. Flip 3 cards</div>
      <p class="qd-step__text">Three quest cards appear face-down. Tap to reveal them one by one.</p>
    </div>
    <div class="qd-step">
      <div class="qd-step__label">3. Go do it</div>
      <p class="qd-step__text">Choose one quest, do it, mark it done. Earn XP and level up.</p>
    </div>
  </div>
</section>

<!-- ===== KEY FEATURES ===== -->
<section class="sc-d" aria-label="QuestDeck features">
  <div style="max-width:var(--max-w);margin:0 auto;padding:48px 24px 24px">
    <h2 style="color:var(--heading);font-size:1.2rem;font-weight:700">Key features</h2>
  </div>
  <div class="qd-features" style="padding-top:0;padding-bottom:48px">
    <div class="qd-feature"><span class="qd-feature__check">✓</span><span>100+ real-world quests — solo, couples, friends</span></div>
    <div class="qd-feature"><span class="qd-feature__check">✓</span><span>Works fully offline — no internet required</span></div>
    <div class="qd-feature"><span class="qd-feature__check">✓</span><span>No account, no login, no tracking</span></div>
    <div class="qd-feature"><span class="qd-feature__check">✓</span><span>XP system, levels, and progress history</span></div>
    <div class="qd-feature"><span class="qd-feature__check">✓</span><span>Easy, medium, and hard difficulty</span></div>
    <div class="qd-feature"><span class="qd-feature__check">✓</span><span>Android — Google Play (coming soon)</span></div>
  </div>
</section>

<!-- ===== ALPHA CTA ===== -->
<section class="qd-cta">
  <p class="qd-cta__label">Alpha testing</p>
  <h2 class="qd-cta__title">Want to be one of the first?</h2>
  <p class="qd-cta__text">QuestDeck is in alpha testing. Write to us and get access.</p>
  <a class="btn-qd" href="mailto:office@bookdragondev.com">office@bookdragondev.com</a>
</section>

<footer>
  <span>©&nbsp;2025&nbsp;Všechna práva vyhrazena</span>
  <a href="questdeck-privacy-policy.html">Privacy policy</a>
  <a href="contact.html">Contact</a>
</footer>

<script>
  const hamburger = document.getElementById('hamburger');
  const menu = document.getElementById('nav-menu');
  hamburger.addEventListener('click', () => {
    const open = menu.classList.toggle('open');
    hamburger.setAttribute('aria-expanded', String(open));
  });
  menu.querySelectorAll('a').forEach(a =>
    a.addEventListener('click', () => menu.classList.remove('open'))
  );
</script>
</body>
</html>
```

- [ ] **Step 2: Verify in browser**

Open `http://localhost:3000/questdeck.html`. Check:
- "QuestDeck" active in nav
- Hero shows "Alpha" badge, title, two paragraphs, "Join alpha testing →" button (opens mail client)
- Email address shown below button
- App icon displayed in right column
- "How it works" section: 3 steps with orange-left-border cards
- "Key features" section: 6 checkmarks in 2 columns
- Alpha CTA section at bottom
- Footer has "Privacy policy" link → `questdeck-privacy-policy.html`
- Mobile: resize to <768px, hero icon stacks below text

- [ ] **Step 3: Commit**

```bash
git add questdeck.html
git commit -m "feat: add QuestDeck landing page"
```

---

## Task 7: Create questdeck-privacy-policy.html

**Files:**
- Create: `questdeck-privacy-policy.html`

Content is exact match to `C:\Projects\AI\Claude\QuestDeck\docs\store\privacy-policy.md`.

- [ ] **Step 1: Create questdeck-privacy-policy.html**

Full content:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Privacy Policy – QuestDeck | Bookdragon Development</title>
  <meta name="description" content="Privacy policy for QuestDeck by Bookdragon Development. QuestDeck does not collect any data." />
  <link rel="canonical" href="https://www.bookdragondev.com/questdeck-privacy-policy.html" />
  <meta property="og:type" content="website" />
  <meta property="og:url" content="https://www.bookdragondev.com/questdeck-privacy-policy.html" />
  <meta property="og:title" content="Privacy Policy – QuestDeck" />
  <meta property="og:description" content="QuestDeck does not collect any data." />
  <meta property="og:site_name" content="Bookdragon Development" />
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap" rel="stylesheet" />
  <link rel="icon" href="images/logo.png" type="image/png" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>

<header>
  <div class="nav-inner">
    <a href="index.html" class="logo">
      <img src="images/logo.png" alt="Bookdragon Development logo" width="40" height="40" />
      Bookdragon Development
    </a>
    <nav>
      <ul id="nav-menu">
        <li><a href="agility-game.html">Agility Game</a></li>
        <li><a href="questdeck.html" class="active">QuestDeck</a></li>
        <li><a href="contact.html">Contact</a></li>
      </ul>
    </nav>
    <button class="hamburger" id="hamburger" aria-label="Menu" aria-expanded="false">
      <span></span><span></span><span></span>
    </button>
  </div>
</header>

<div class="page-header" style="border-bottom:1px solid var(--border)">
  <div style="max-width:var(--max-w);margin:0 auto;padding:0 24px">
    <p style="color:var(--accent-qd);font-size:0.78rem;font-weight:700;letter-spacing:.08em;text-transform:uppercase;margin-bottom:8px">QuestDeck</p>
    <h1 style="color:var(--heading);font-size:clamp(1.6rem,3vw,2.2rem);font-weight:700">Privacy Policy</h1>
    <p style="color:var(--muted);font-size:0.82rem;margin-top:8px">Last updated: May 15, 2026</p>
  </div>
</div>

<div class="page-content page-content--qd">

  <div class="highlight-box">
    <p class="highlight-box__title">QuestDeck does not collect any data.</p>
    <p class="highlight-box__sub">The app works entirely offline and never transmits any information.</p>
  </div>

  <p>This Privacy Policy explains how QuestDeck ("the app") handles information.</p>

  <h2>Data Collection</h2>
  <p>QuestDeck does not collect, store, transmit, or share any personal information. The app does not require an account, login, or registration of any kind.</p>

  <h2>Data Stored on Your Device</h2>
  <p>The app stores the following data locally on your device only:</p>
  <ul>
    <li>Completed quests and their timestamps</li>
    <li>Your XP points and level</li>
    <li>App settings (sound and haptics preferences)</li>
    <li>Unlocked quest packs</li>
  </ul>
  <p>This data never leaves your device. It is stored using your device's local storage and is not accessible to us or any third party.</p>

  <h2>Network Access</h2>
  <p>QuestDeck does not make any network requests. The app works entirely offline after installation.</p>

  <h2>Third-Party Services</h2>
  <p>QuestDeck does not use any third-party services, SDKs, analytics tools, advertising networks, or crash reporting services.</p>

  <h2>Children's Privacy</h2>
  <p>QuestDeck does not collect any information from anyone, including children under the age of 13.</p>

  <h2>Changes to This Policy</h2>
  <p>If the app's data practices change in a future version, this policy will be updated accordingly.</p>

  <h2>Contact</h2>
  <p>If you have any questions about this Privacy Policy, contact us at: <a href="mailto:tomas.hutyra@gmail.com">tomas.hutyra@gmail.com</a></p>

  <p style="margin-top:32px"><a href="questdeck.html">← Back to QuestDeck</a></p>

</div>

<footer>
  <span>©&nbsp;2025&nbsp;Všechna práva vyhrazena</span>
  <a href="questdeck-privacy-policy.html">Privacy policy</a>
  <a href="contact.html">Contact</a>
</footer>

<script>
  const hamburger = document.getElementById('hamburger');
  const menu = document.getElementById('nav-menu');
  hamburger.addEventListener('click', () => {
    const open = menu.classList.toggle('open');
    hamburger.setAttribute('aria-expanded', String(open));
  });
  menu.querySelectorAll('a').forEach(a =>
    a.addEventListener('click', () => menu.classList.remove('open'))
  );
</script>
</body>
</html>
```

- [ ] **Step 2: Verify content matches source MD**

Compare sections against `C:\Projects\AI\Claude\QuestDeck\docs\store\privacy-policy.md`:
- "QuestDeck does not collect any data." ✓ (in highlight box)
- All 7 section headings present ✓
- Bullet list under "Data Stored on Your Device" has all 4 items ✓
- Contact email is `tomas.hutyra@gmail.com` ✓
- Last updated date: May 15, 2026 ✓

- [ ] **Step 3: Verify in browser**

Open `http://localhost:3000/questdeck-privacy-policy.html`. Check:
- "QuestDeck" label in orange above page title
- Orange highlight box at top with "does not collect any data"
- Links (email, back to QuestDeck) render in `#FF8C42`
- "← Back to QuestDeck" links to `questdeck.html`

- [ ] **Step 4: Commit**

```bash
git add questdeck-privacy-policy.html
git commit -m "feat: add QuestDeck privacy policy page"
```

---

## Task 8: Update netlify.toml and sitemap.xml

**Files:**
- Modify: `netlify.toml`
- Modify: `sitemap.xml`

- [ ] **Step 1: Add redirects to netlify.toml**

Append to `netlify.toml`:

```toml
[[redirects]]
  from = "/agility-game"
  to = "/agility-game.html"
  status = 301

[[redirects]]
  from = "/questdeck"
  to = "/questdeck.html"
  status = 301

[[redirects]]
  from = "/questdeck-privacy-policy"
  to = "/questdeck-privacy-policy.html"
  status = 301
```

- [ ] **Step 2: Replace sitemap.xml**

Full content of `sitemap.xml`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://www.bookdragondev.com/</loc>
    <lastmod>2026-05-15</lastmod>
    <changefreq>monthly</changefreq>
    <priority>1.0</priority>
  </url>
  <url>
    <loc>https://www.bookdragondev.com/agility-game.html</loc>
    <lastmod>2026-05-15</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.9</priority>
  </url>
  <url>
    <loc>https://www.bookdragondev.com/questdeck.html</loc>
    <lastmod>2026-05-15</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.9</priority>
  </url>
  <url>
    <loc>https://www.bookdragondev.com/privacy-policy.html</loc>
    <lastmod>2026-05-15</lastmod>
    <changefreq>yearly</changefreq>
    <priority>0.3</priority>
  </url>
  <url>
    <loc>https://www.bookdragondev.com/questdeck-privacy-policy.html</loc>
    <lastmod>2026-05-15</lastmod>
    <changefreq>yearly</changefreq>
    <priority>0.3</priority>
  </url>
  <url>
    <loc>https://www.bookdragondev.com/contact.html</loc>
    <lastmod>2026-05-15</lastmod>
    <changefreq>yearly</changefreq>
    <priority>0.5</priority>
  </url>
</urlset>
```

- [ ] **Step 3: Commit**

```bash
git add netlify.toml sitemap.xml
git commit -m "feat: update netlify redirects and sitemap for two-app portfolio"
```

---

## Task 9: Final cross-page verification

- [ ] **Step 1: Check all nav links work**

From `http://localhost:3000`:
- Logo → `index.html` ✓
- "Agility Game" → `agility-game.html` ✓
- "QuestDeck" → `questdeck.html` ✓
- "Contact" → `contact.html` ✓

From `agility-game.html`:
- Active state on "Agility Game" ✓
- "QuestDeck" navigates correctly ✓

From `questdeck.html`:
- Active state on "QuestDeck" ✓

From `questdeck-privacy-policy.html`:
- Active state on "QuestDeck" ✓
- "← Back to QuestDeck" works ✓

- [ ] **Step 2: Check footer links on all pages**

| Page | Footer privacy link |
|---|---|
| `index.html` | no privacy link (company page) |
| `agility-game.html` | → `privacy-policy.html` |
| `privacy-policy.html` | → `privacy-policy.html` |
| `questdeck.html` | → `questdeck-privacy-policy.html` |
| `questdeck-privacy-policy.html` | → `questdeck-privacy-policy.html` |
| `contact.html` | → `privacy-policy.html` (kept from original) |

- [ ] **Step 3: Mobile hamburger menu check**

Resize browser to 375px width. On each page:
- Hamburger visible
- Tapping opens menu with all 3 links
- Tapping a link closes menu and navigates

- [ ] **Step 4: Final commit**

```bash
git add -A
git status
git commit -m "feat: complete two-app portfolio restructure"
```
