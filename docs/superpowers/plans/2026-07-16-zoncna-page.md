# Zoncna Landing Page Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add ZONCNA as a third product on the Bookdragon Development portfolio site: a Czech-language landing page, a Czech-language privacy policy page, and full integration into the shared nav, homepage, sitemap, and redirects.

**Architecture:** Static HTML/CSS, no build step. Follows the existing per-app theming convention (`body.<app>-page` scoping in `style.css`) established by `body.ag-page` and `body.qd-page`. No JavaScript framework, no test runner — verification is done via `grep`-based structural checks (every internal link/image reference resolves to a real file) plus a manual browser pass at the end.

**Tech Stack:** Plain HTML5, CSS custom properties, vanilla JS (hamburger menu snippet only). Deployed via Netlify, publish dir = repo root.

## Global Constraints

- Page content (hero, sections, CTA copy) on `zoncna.html` and `zoncna-privacy-policy.html` is in **Czech**. Shared chrome — header nav labels, footer links ("Privacy policy", "Contact"), footer copyright line — stays in **English**, matching every other page on the site.
- Footer copyright line must be copied verbatim: `&copy;&nbsp;2025&nbsp;Bookdragon Development` (English only, literal "2025" — this is the standardized text across the whole site, do not change the year or translate it).
- No `<link>` Google Fonts tags on new pages — Plus Jakarta Sans is loaded once via `@import` in `style.css`. Trust the existing `@import`.
- The hamburger-menu `<script>` snippet at the bottom of every page must be byte-for-byte identical to the one already in `questdeck.html`.
- CTA target for ZONCNA beta signup: `https://forms.gle/8CmkTbt7TWKeRiad7` (not a `mailto:` link).
- Contact email shown as a fallback: `office@bookdragondev.com`.
- Canonical domain: `https://www.bookdragondev.com/`.
- Source assets live in `c:\Projects\AI\Claude\Zoncna\` (a separate, unrelated git repo — read-only for this plan, never modify files there).

---

### Task 1: Copy image assets into `images/`

**Files:**
- Create: `images/zoncna-icon.png` (copy of `c:\Projects\AI\Claude\Zoncna\mobile\resources\icon.png`)
- Create: `images/zoncna-FeaturedGraphic.png` (copy of `c:\Projects\AI\Claude\Zoncna\Doc\FeatureGraphic1024x500.png`)
- Create: `images/zoncna-screenshot-1.jpg` (copy of `c:\Projects\AI\Claude\Zoncna\Doc\Screenshot_20260716_071154_ZONCNA.jpg`)
- Create: `images/zoncna-screenshot-2.jpg` (copy of `c:\Projects\AI\Claude\Zoncna\Doc\Screenshot_20260716_071309_ZONCNA.jpg`)

**Interfaces:**
- Consumes: nothing (first task)
- Produces: four image files under `images/` that Tasks 3, 4, and 6 reference by exact path (`images/zoncna-icon.png`, `images/zoncna-FeaturedGraphic.png`, `images/zoncna-screenshot-1.jpg`, `images/zoncna-screenshot-2.jpg`)

- [ ] **Step 1: Copy the four source files**

```bash
cp "c:/Projects/AI/Claude/Zoncna/mobile/resources/icon.png" "c:/Projects/Web/www.bookdragondev.com/images/zoncna-icon.png"
cp "c:/Projects/AI/Claude/Zoncna/Doc/FeatureGraphic1024x500.png" "c:/Projects/Web/www.bookdragondev.com/images/zoncna-FeaturedGraphic.png"
cp "c:/Projects/AI/Claude/Zoncna/Doc/Screenshot_20260716_071154_ZONCNA.jpg" "c:/Projects/Web/www.bookdragondev.com/images/zoncna-screenshot-1.jpg"
cp "c:/Projects/AI/Claude/Zoncna/Doc/Screenshot_20260716_071309_ZONCNA.jpg" "c:/Projects/Web/www.bookdragondev.com/images/zoncna-screenshot-2.jpg"
```

- [ ] **Step 2: Verify all four files exist and are non-empty**

Run: `ls -la images/zoncna-icon.png images/zoncna-FeaturedGraphic.png images/zoncna-screenshot-1.jpg images/zoncna-screenshot-2.jpg`
Expected: four lines listed, each with a size greater than 0 bytes (no "No such file" errors).

- [ ] **Step 3: Commit**

```bash
git add images/zoncna-icon.png images/zoncna-FeaturedGraphic.png images/zoncna-screenshot-1.jpg images/zoncna-screenshot-2.jpg
git commit -m "feat: add Zoncna image assets"
```

---

### Task 2: Add `body.zn-page` theme to `style.css`

**Files:**
- Modify: `style.css:19` (root variable block)
- Modify: `style.css:1086` (homepage `.hpcard__cta` variants block)
- Modify: `style.css:1278` (`body.home` hpcard CTA overrides block)
- Modify: `style.css:1835` (append new theme block before the RESPONSIVE section)
- Modify: `style.css:1846` (768px responsive block)
- Modify: `style.css:1852` (480px responsive block)

**Interfaces:**
- Consumes: nothing structural (pure CSS addition)
- Produces: CSS custom properties `--accent-zn` / `--accent-zn-h` (root-level, used by the homepage card in Task 6); `body.zn-page` theme scope with `--accent-zn-blue`; component classes `.zn-hero`, `.zn-hero__inner`, `.zn-hero__label`, `.zn-hero__title`, `.zn-hero__sub`, `.zn-hero__cta`, `.zn-hero__note`, `.zn-featured-img`, `.zn-featured-caption`, `.zn-section-header`, `.zn-section-header__label`, `.zn-section-header__title`, `.zn-steps`, `.zn-step`, `.zn-step__num`, `.zn-step__title`, `.zn-step__text`, `.zn-features`, `.zn-feat`, `.zn-feat__icon`, `.zn-screens`, `.zn-cta`, `.zn-cta__title`, `.zn-cta__text`, `.zn-cta__btn`, `.hpcard__cta--zn` — all consumed by Tasks 3, 4, and 6.

- [ ] **Step 1: Add root-level accent variables**

In `style.css`, find (around line 18-19):

```css
  --accent-qd:  #FF8C42;
  --accent-qd-h: #e07a32;
```

Replace with:

```css
  --accent-qd:  #FF8C42;
  --accent-qd-h: #e07a32;
  --accent-zn:  #3b8bd6;
  --accent-zn-h: #2f70ad;
```

- [ ] **Step 2: Add the homepage card CTA variant (component-level rule)**

In `style.css`, find (around line 1079-1086):

```css
.hpcard__cta--qd {
  background: var(--accent-qd);
  color: #fff;
}
.hpcard__cta--qd:hover {
  background: var(--accent-qd-h);
  box-shadow: 0 4px 18px rgba(255,140,66,0.42);
}
```

Replace with:

```css
.hpcard__cta--qd {
  background: var(--accent-qd);
  color: #fff;
}
.hpcard__cta--qd:hover {
  background: var(--accent-qd-h);
  box-shadow: 0 4px 18px rgba(255,140,66,0.42);
}

.hpcard__cta--zn {
  background: var(--accent-zn);
  color: #fff;
}
.hpcard__cta--zn:hover {
  background: var(--accent-zn-h);
  box-shadow: 0 4px 18px rgba(59,139,214,0.42);
}
```

- [ ] **Step 3: Add the `body.home` override for the same button**

In `style.css`, find (around line 1274-1278):

```css
body.home .hpcard__cta--qd { background: var(--accent-qd); }
body.home .hpcard__cta--qd:hover {
  background: var(--accent-qd-h);
  box-shadow: 0 4px 18px rgba(255,140,66,0.40);
}
```

Replace with:

```css
body.home .hpcard__cta--qd { background: var(--accent-qd); }
body.home .hpcard__cta--qd:hover {
  background: var(--accent-qd-h);
  box-shadow: 0 4px 18px rgba(255,140,66,0.40);
}

body.home .hpcard__cta--zn { background: var(--accent-zn); }
body.home .hpcard__cta--zn:hover {
  background: var(--accent-zn-h);
  box-shadow: 0 4px 18px rgba(59,139,214,0.40);
}
```

- [ ] **Step 4: Append the full ZONCNA page theme block**

In `style.css`, find the end of the contact-page block, right before the responsive comment header (around line 1834-1837):

```css
body.contact-page footer { background: var(--bg-dark2); border-top-color: var(--border); }
body.contact-page footer a:hover { color: var(--heading); }

/* ==========================================
   RESPONSIVE — new page components
   ========================================== */
```

Replace with:

```css
body.contact-page footer { background: var(--bg-dark2); border-top-color: var(--border); }
body.contact-page footer a:hover { color: var(--heading); }

/* ==========================================
   ZONCNA PAGE  (body.zn-page)
   Theme: Dark Navy · Radar Noir · Amber Accent
   Inspired by: the game's own in-app UI —
   navy-dark background, amber title rule/CTA,
   radar-blue GPS pin and rainbow radar bloom
   ========================================== */

body.zn-page {
  --bg-dark:   #11172a;
  --bg-dark2:  #0b0f1d;
  --bg-alt:    #1a2338;
  --accent:    #f0b429;
  --accent-h:  #d69a1a;
  --accent-zn-blue: #3b8bd6;
  --text:      #b9c2d6;
  --heading:   #f4f1e8;
  --muted:     #6b7690;
  --border:    #232e48;
  --btn-bg:    #f0b429;
  --btn-text:  #11172a;
}

body.zn-page nav a:hover,
body.zn-page nav a.active {
  background: rgba(240,180,41,0.10);
  color: var(--accent);
}

/* ---- ZN Hero ---- */
.zn-hero {
  background: var(--bg-dark);
  border-bottom: 1px solid var(--border);
}

.zn-hero__inner {
  max-width: var(--max-w);
  margin: 0 auto;
  padding: 64px 24px;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 52px;
  align-items: center;
}

.zn-hero__label {
  display: inline-block;
  background: var(--accent);
  color: var(--btn-text);
  font-size: 0.72rem;
  font-weight: 700;
  letter-spacing: .12em;
  text-transform: uppercase;
  padding: 4px 12px;
  border-radius: 100px;
  margin-bottom: 20px;
}

.zn-hero__title {
  color: var(--heading);
  font-size: clamp(2.2rem, 4.5vw, 3.4rem);
  font-weight: 800;
  line-height: 1.1;
  letter-spacing: -.03em;
  margin-bottom: 20px;
}

.zn-hero__title em {
  font-style: normal;
  color: var(--accent);
}

.zn-hero__sub {
  color: var(--text);
  font-size: 1rem;
  line-height: 1.78;
  margin-bottom: 32px;
  max-width: 440px;
}

.zn-hero__cta {
  display: inline-block;
  background: var(--accent);
  color: var(--btn-text);
  padding: 14px 30px;
  border-radius: 6px;
  font-weight: 700;
  font-size: 0.95rem;
  box-shadow: 0 4px 20px rgba(240,180,41,0.30);
  transition: background .15s, transform .18s, box-shadow .18s;
}
.zn-hero__cta:hover {
  background: var(--accent-h);
  transform: translateY(-2px);
  box-shadow: 0 8px 28px rgba(240,180,41,0.42);
}

.zn-hero__note {
  display: block;
  margin-top: 14px;
  font-size: 0.82rem;
  color: var(--muted);
}
.zn-hero__note a { color: var(--accent); font-weight: 600; }
.zn-hero__note a:hover { text-decoration: underline; }

.zn-featured-img {
  border-radius: 18px;
  overflow: hidden;
  box-shadow: 0 20px 60px rgba(0,0,0,0.45);
}
.zn-featured-img img { width: 100%; height: auto; display: block; }

.zn-featured-caption {
  margin-top: 8px;
  font-size: 0.72rem;
  color: var(--muted);
  text-align: right;
}

/* ---- ZN Section Header ---- */
.zn-section-header {
  max-width: var(--max-w);
  margin: 0 auto;
  padding: 64px 24px 0;
}
.zn-section-header__label {
  color: var(--accent);
  font-size: 0.75rem;
  font-weight: 700;
  letter-spacing: .14em;
  text-transform: uppercase;
  margin-bottom: 8px;
}
.zn-section-header__title {
  color: var(--heading);
  font-size: clamp(1.5rem, 3vw, 2rem);
  font-weight: 800;
  letter-spacing: -.02em;
}

/* ---- ZN Steps ---- */
.zn-steps {
  max-width: var(--max-w);
  margin: 0 auto;
  padding: 28px 24px 64px;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
}

.zn-step {
  background: var(--bg-alt);
  border: 1px solid var(--border);
  border-radius: 16px;
  padding: 30px 24px;
  transition: transform .22s, box-shadow .22s;
}
.zn-step:hover {
  transform: translateY(-5px);
  box-shadow: 0 14px 40px rgba(0,0,0,0.35);
}

.zn-step__num {
  width: 38px; height: 38px;
  background: var(--accent);
  color: var(--btn-text);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 800;
  font-size: 0.9rem;
  margin-bottom: 16px;
}

.zn-step__title {
  color: var(--heading);
  font-weight: 700;
  font-size: 1.05rem;
  margin-bottom: 8px;
}

.zn-step__text {
  color: var(--text);
  font-size: 0.88rem;
  line-height: 1.7;
}

/* ---- ZN Features ---- */
.zn-features {
  max-width: var(--max-w);
  margin: 0 auto;
  padding: 28px 24px 64px;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 10px;
}

.zn-feat {
  display: flex;
  gap: 12px;
  align-items: flex-start;
  background: var(--bg-alt);
  border: 1px solid var(--border);
  border-radius: 10px;
  padding: 14px 18px;
  font-size: 0.88rem;
  color: var(--text);
  transition: border-color .15s;
}
.zn-feat:hover { border-color: var(--accent); }

.zn-feat__icon {
  width: 20px; height: 20px;
  background: var(--accent-zn-blue);
  color: #fff;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.6rem;
  font-weight: 900;
  flex-shrink: 0;
  margin-top: 1px;
}

/* ---- ZN Screenshots strip ---- */
.zn-screens {
  max-width: var(--max-w);
  margin: 0 auto;
  padding: 0 24px 64px;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
}
.zn-screens img {
  width: 100%;
  height: auto;
  display: block;
  border-radius: 12px;
  border: 1px solid var(--border);
}

/* ---- ZN CTA ---- */
.zn-cta {
  background: var(--accent);
  padding: 80px 24px;
  text-align: center;
}

.zn-cta__title {
  color: var(--btn-text);
  font-size: clamp(1.6rem, 3vw, 2.2rem);
  font-weight: 800;
  letter-spacing: -.025em;
  margin-bottom: 12px;
}

.zn-cta__text {
  color: rgba(17,23,42,0.78);
  font-size: 0.95rem;
  margin-bottom: 28px;
}

.zn-cta__btn {
  display: inline-block;
  background: var(--bg-dark);
  color: var(--accent);
  padding: 13px 30px;
  border-radius: 6px;
  font-weight: 800;
  font-size: 0.95rem;
  transition: transform .18s, box-shadow .18s;
}
.zn-cta__btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 24px rgba(0,0,0,0.35);
}

body.zn-page footer { background: var(--bg-dark2); border-top-color: var(--border); }
body.zn-page footer a:hover { color: var(--accent); }

/* zn-page privacy policy */
body.zn-page .page-header { background: var(--bg-dark); border-bottom-color: var(--border); }
body.zn-page .page-content { color: var(--text); }
body.zn-page .page-content h2 { color: var(--accent); }
body.zn-page .page-content a { color: var(--accent); }
body.zn-page .highlight-box { background: var(--bg-alt); border-left-color: var(--accent); }

/* ==========================================
   RESPONSIVE — new page components
   ========================================== */
```

- [ ] **Step 5: Add ZONCNA rules to the existing 768px responsive block**

In `style.css`, find (around line 1840-1847):

```css
@media (max-width: 768px) {
  .qd-hero-v2__inner  { grid-template-columns: 1fr; gap: 32px; padding: 40px 16px; }
  .qd-steps-v2        { grid-template-columns: 1fr; padding: 24px 16px 48px; }
  .qd-features-v2     { grid-template-columns: 1fr; padding: 24px 16px 48px; }
  .qd-section-header  { padding: 48px 16px 0; }
  .qd-featured-img    { max-width: 480px; }
  .cta-store-buttons  { flex-direction: column; align-items: center; }
}
```

Replace with:

```css
@media (max-width: 768px) {
  .qd-hero-v2__inner  { grid-template-columns: 1fr; gap: 32px; padding: 40px 16px; }
  .qd-steps-v2        { grid-template-columns: 1fr; padding: 24px 16px 48px; }
  .qd-features-v2     { grid-template-columns: 1fr; padding: 24px 16px 48px; }
  .qd-section-header  { padding: 48px 16px 0; }
  .qd-featured-img    { max-width: 480px; }
  .cta-store-buttons  { flex-direction: column; align-items: center; }
  .zn-hero__inner     { grid-template-columns: 1fr; gap: 32px; padding: 40px 16px; }
  .zn-steps           { grid-template-columns: 1fr; padding: 24px 16px 48px; }
  .zn-features        { grid-template-columns: 1fr; padding: 24px 16px 48px; }
  .zn-screens         { grid-template-columns: 1fr; padding: 0 16px 48px; }
  .zn-section-header  { padding: 48px 16px 0; }
}
```

- [ ] **Step 6: Add ZONCNA rules to the existing 480px responsive block**

In `style.css`, find (around line 1849-1853):

```css
@media (max-width: 480px) {
  .qd-cta-v2 { padding: 56px 16px; }
  .qd-cta-v2__btn { display: block; text-align: center; }
  .qd-hero-v2__sub { max-width: 100%; }
}
```

Replace with:

```css
@media (max-width: 480px) {
  .qd-cta-v2 { padding: 56px 16px; }
  .qd-cta-v2__btn { display: block; text-align: center; }
  .qd-hero-v2__sub { max-width: 100%; }
  .zn-cta { padding: 56px 16px; }
  .zn-cta__btn { display: block; text-align: center; }
  .zn-hero__sub { max-width: 100%; }
}
```

- [ ] **Step 7: Verify every new selector is present**

Run:
```bash
grep -c "body.zn-page\|\.zn-hero\|\.zn-step\|\.zn-feat\|\.zn-cta\|\.zn-screens\|hpcard__cta--zn\|accent-zn" style.css
```
Expected: a number greater than 30 (one hit per occurrence across the definitions and responsive rules added above — confirms nothing was skipped).

- [ ] **Step 8: Commit**

```bash
git add style.css
git commit -m "feat: add Zoncna dark-navy theme to style.css"
```

---

### Task 3: Create `zoncna.html`

**Files:**
- Create: `zoncna.html`

**Interfaces:**
- Consumes: `images/zoncna-icon.png`, `images/zoncna-FeaturedGraphic.png`, `images/zoncna-screenshot-1.jpg`, `images/zoncna-screenshot-2.jpg` (Task 1); `body.zn-page` theme and `.zn-*` classes (Task 2)
- Produces: `zoncna.html` — the URL every nav link, homepage card CTA, and sitemap entry in later tasks points to

- [ ] **Step 1: Write the file**

```html
<!DOCTYPE html>
<html lang="cs">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <title>ZONCNA – simulátor brněnských srážek | Bookdragon Development</title>
  <meta name="description" content="ZONCNA je ironický radar-simulátor brněnských srážek. Táhni prstem, uhýbej dešti a udrž Brno v suchu. Closed beta na prohlížeči i Androidu." />
  <link rel="canonical" href="https://www.bookdragondev.com/zoncna.html" />

  <!-- Open Graph -->
  <meta property="og:type" content="website" />
  <meta property="og:url" content="https://www.bookdragondev.com/zoncna.html" />
  <meta property="og:title" content="ZONCNA – simulátor brněnských srážek" />
  <meta property="og:description" content="Táhni prstem. Uhni s celým městem. Ironický radar-simulátor brněnských srážek." />
  <meta property="og:image" content="https://www.bookdragondev.com/images/zoncna-FeaturedGraphic.png" />
  <meta property="og:site_name" content="Bookdragon Development" />

  <!-- Twitter Card -->
  <meta name="twitter:card" content="summary_large_image" />
  <meta name="twitter:title" content="ZONCNA – simulátor brněnských srážek" />
  <meta name="twitter:description" content="Táhni prstem. Uhni s celým městem. Ironický radar-simulátor brněnských srážek." />
  <meta name="twitter:image" content="https://www.bookdragondev.com/images/zoncna-FeaturedGraphic.png" />

  <!-- Schema.org -->
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "MobileApplication",
    "name": "ZONCNA",
    "description": "Ironický radar-simulátor brněnských srážek. Táhni celým městem a uhýbej dešti, dokud to jde.",
    "url": "https://www.bookdragondev.com/zoncna.html",
    "image": "https://www.bookdragondev.com/images/zoncna-FeaturedGraphic.png",
    "operatingSystem": "Web, Android",
    "applicationCategory": "GameApplication",
    "publisher": {
      "@type": "Organization",
      "name": "Bookdragon Development",
      "email": "office@bookdragondev.com",
      "url": "https://www.bookdragondev.com/"
    }
  }
  </script>

  <link rel="icon" href="images/logo.png" type="image/png" />
  <link rel="stylesheet" href="style.css" />
</head>
<body class="zn-page">

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
        <li><a href="zoncna.html" class="active">Zoncna</a></li>
        <li><a href="contact.html">Contact</a></li>
      </ul>
    </nav>
    <button class="hamburger" id="hamburger" aria-label="Menu" aria-expanded="false">
      <span></span><span></span><span></span>
    </button>
  </div>
</header>

<!-- HERO -->
<section class="zn-hero">
  <div class="zn-hero__inner">
    <div>
      <span class="zn-hero__label">Closed beta &middot; Prohlížeč + Android</span>
      <h1 class="zn-hero__title">Táhni prstem.<br>Uhni s celým <em>městem</em>.</h1>
      <p class="zn-hero__sub">ZONCNA je ironický radar-simulátor brněnských srážek. Neovládáš mrak, ale celé město — cílem je udržet Brno v suchu, zatímco se od okrajů mapy valí barevné peklo. Čím dél vydržíš suchý, tím vyšší suchoskóre.</p>
      <a class="zn-hero__cta" href="https://forms.gle/8CmkTbt7TWKeRiad7">Přihlásit se do closed testu &rarr;</a>
      <span class="zn-hero__note">nebo napiš na <a href="mailto:office@bookdragondev.com">office@bookdragondev.com</a></span>
    </div>
    <div>
      <div class="zn-featured-img">
        <img src="images/zoncna-FeaturedGraphic.png" alt="ZONCNA — simulátor brněnských srážek, radarová mapa Brna s dešťovými mraky" width="1024" height="500" loading="eager" />
      </div>
      <p class="zn-featured-caption">Mapové podklady: &copy; přispěvatelé OpenStreetMap, &copy; CARTO</p>
    </div>
  </div>
</section>

<!-- HOW IT WORKS -->
<section class="sc-d" aria-label="Jak ZONCNA funguje">
  <div class="zn-section-header">
    <p class="zn-section-header__label">Jednoduché ovládání</p>
    <h2 class="zn-section-header__title">Jak to funguje</h2>
  </div>
  <div class="zn-steps">
    <div class="zn-step">
      <div class="zn-step__num">1</div>
      <div class="zn-step__title">Táhni Brno po radaru</div>
      <p class="zn-step__text">Uhýbej srážkám tažením prstu nebo myši — neovládáš mrak, ale samotné město.</p>
    </div>
    <div class="zn-step">
      <div class="zn-step__num">2</div>
      <div class="zn-step__title">Sleduj promočení</div>
      <p class="zn-step__text">Čím silnější odraz zasáhne město, tím rychleji roste ukazatel promočení. Mimo déšť město pomalu vysychá.</p>
    </div>
    <div class="zn-step">
      <div class="zn-step__num">3</div>
      <div class="zn-step__title">Zapiš se do kroniky sucha</div>
      <p class="zn-step__text">V top 10 zadáš jméno a poměříš se s ostatními suchaři na lokálním žebříčku.</p>
    </div>
  </div>
</section>

<!-- FEATURES -->
<section class="sc-cd" aria-label="Vlastnosti ZONCNA">
  <div class="zn-section-header">
    <p class="zn-section-header__label">Co v tom najdeš</p>
    <h2 class="zn-section-header__title">Seriózní radar, absurdní obsah</h2>
  </div>
  <div class="zn-features">
    <div class="zn-feat">
      <span class="zn-feat__icon">&#10003;</span>
      <span>Skutečný mapový podklad Brna a okolí (~30 km)</span>
    </div>
    <div class="zn-feat">
      <span class="zn-feat__icon">&#10003;</span>
      <span>Endless mode s plynule rostoucí obtížností</span>
    </div>
    <div class="zn-feat">
      <span class="zn-feat__icon">&#10003;</span>
      <span>Lokální kronika sucha (top 10) &mdash; žádný účet potřeba</span>
    </div>
    <div class="zn-feat">
      <span class="zn-feat__icon">&#10003;</span>
      <span>Nulový sběr dat, nulové reklamy</span>
    </div>
    <div class="zn-feat">
      <span class="zn-feat__icon">&#10003;</span>
      <span>Ironický, suchý meteorologický tón &mdash; brněnský hantec vítán</span>
    </div>
    <div class="zn-feat">
      <span class="zn-feat__icon">&#10003;</span>
      <span>Prohlížeč (PC i mobil) + Android (beta)</span>
    </div>
  </div>
</section>

<!-- SCREENSHOTS -->
<section class="sc-d" aria-label="Snímky ze hry">
  <div class="zn-section-header">
    <p class="zn-section-header__label">Živě z radaru</p>
    <h2 class="zn-section-header__title">Jak to vypadá</h2>
  </div>
  <div class="zn-screens">
    <img src="images/zoncna-screenshot-1.jpg" alt="ZONCNA — úvodní obrazovka s tlačítkem Rozežeň to" loading="lazy" />
    <img src="images/zoncna-screenshot-2.jpg" alt="ZONCNA — hraní, Brno uhýbá srážkám na radaru" loading="lazy" />
  </div>
</section>

<!-- CTA -->
<section class="zn-cta">
  <h2 class="zn-cta__title">Chceš být první suchař?</h2>
  <p class="zn-cta__text">ZONCNA sbírá lidi do closed testu. Přihlas se přes formulář a pomoz nám hru doladit.</p>
  <a class="zn-cta__btn" href="https://forms.gle/8CmkTbt7TWKeRiad7">Vyplnit formulář &rarr;</a>
</section>

<footer>
  <span>&copy;&nbsp;2025&nbsp;Bookdragon Development</span>
  <a href="zoncna-privacy-policy.html">Privacy policy</a>
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

- [ ] **Step 2: Verify the file is well-formed and self-consistent**

Run:
```bash
grep -c "<section" zoncna.html
grep -c "</section>" zoncna.html
grep -o 'class="zn-[a-z_-]*"' zoncna.html | sort -u
```
Expected: the two `<section>`/`</section>` counts match (5 each); the class list includes `zn-cta`, `zn-cta__btn`, `zn-cta__text`, `zn-cta__title`, `zn-featured-caption`, `zn-featured-img`, `zn-feat`, `zn-feat__icon`, `zn-features`, `zn-hero`, `zn-hero__cta`, `zn-hero__inner`, `zn-hero__label`, `zn-hero__note`, `zn-hero__sub`, `zn-hero__title`, `zn-screens`, `zn-section-header`, `zn-section-header__label`, `zn-section-header__title`, `zn-step`, `zn-step__num`, `zn-step__text`, `zn-step__title` — every one of these must already exist in `style.css` from Task 2 (spot-check a couple with `grep "zn-step__num" style.css`).

- [ ] **Step 3: Commit**

```bash
git add zoncna.html
git commit -m "feat: add ZONCNA landing page"
```

---

### Task 4: Create `zoncna-privacy-policy.html`

**Files:**
- Create: `zoncna-privacy-policy.html`

**Interfaces:**
- Consumes: `images/zoncna-icon.png` (Task 1); `body.zn-page` theme, generic `.page-header`/`.page-content`/`.pp-brand*`/`.highlight-box*` classes already shared by `questdeck-privacy-policy.html` (Task 2 adds the `body.zn-page` overrides for these)
- Produces: `zoncna-privacy-policy.html` — the URL the footer link on `zoncna.html` (Task 3) and the sitemap entry (Task 7) point to

- [ ] **Step 1: Write the file**

```html
<!DOCTYPE html>
<html lang="cs">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Zásady ochrany osobních údajů – ZONCNA | Bookdragon Development</title>
  <meta name="description" content="Zásady ochrany osobních údajů pro ZONCNA od Bookdragon Development. ZONCNA nesbírá žádná osobní data." />
  <link rel="canonical" href="https://www.bookdragondev.com/zoncna-privacy-policy.html" />
  <meta property="og:type" content="website" />
  <meta property="og:url" content="https://www.bookdragondev.com/zoncna-privacy-policy.html" />
  <meta property="og:title" content="Zásady ochrany osobních údajů – ZONCNA" />
  <meta property="og:description" content="ZONCNA nesbírá žádná osobní data." />
  <meta property="og:site_name" content="Bookdragon Development" />
  <link rel="icon" href="images/logo.png" type="image/png" />
  <link rel="stylesheet" href="style.css" />
</head>
<body class="zn-page">

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
        <li><a href="zoncna.html" class="active">Zoncna</a></li>
        <li><a href="contact.html">Contact</a></li>
      </ul>
    </nav>
    <button class="hamburger" id="hamburger" aria-label="Menu" aria-expanded="false">
      <span></span><span></span><span></span>
    </button>
  </div>
</header>

<div class="page-header">
  <div class="pp-brand">
    <img src="images/zoncna-icon.png" alt="ZONCNA icon" class="pp-brand__icon" />
    <div>
      <span class="pp-brand__label">ZONCNA</span>
      <h1 class="pp-brand__title">Zásady ochrany osobních údajů</h1>
    </div>
  </div>
</div>

<div class="page-content">

  <div class="highlight-box">
    <p class="highlight-box__title">ZONCNA neshromažďuje žádná osobní data.</p>
    <p class="highlight-box__sub">Hra funguje offline a data z tvého zařízení nikam neodesílá.</p>
  </div>

  <p>Tyto zásady ochrany osobních údajů vysvětlují, jak ZONCNA ("aplikace") nakládá s informacemi.</p>

  <h2>Sběr dat</h2>
  <p>ZONCNA nesbírá, neukládá, nepřenáší ani nesdílí žádné osobní údaje. Aplikace nevyžaduje účet, přihlášení ani žádnou registraci.</p>

  <h2>Data uložená v zařízení</h2>
  <p>Aplikace ukládá následující data pouze lokálně na tvém zařízení:</p>
  <ul>
    <li>Kroniku sucha (jméno a skóre v top 10 žebříčku)</li>
    <li>Odemčené odznaky a herní postup</li>
    <li>Nastavení zvuku</li>
  </ul>
  <p>Tato data nikdy neopouští tvé zařízení. Jsou uložena pomocí lokálního úložiště prohlížeče (localStorage) a nejsou přístupná nám ani žádné třetí straně.</p>

  <h2>Síťový přístup</h2>
  <p>ZONCNA nevytváří žádné síťové požadavky za běhu. Mapový podklad je statický a je součástí instalace/buildu — hra po instalaci funguje zcela offline.</p>

  <h2>Služby třetích stran</h2>
  <p>ZONCNA nepoužívá žádné SDK třetích stran, reklamní sítě, analytické nástroje ani nástroje pro hlášení pádů.</p>

  <h2>Ochrana dětí</h2>
  <p>ZONCNA nesbírá žádné informace od nikoho, včetně dětí mladších 13 let.</p>

  <h2>Změny těchto zásad</h2>
  <p>Pokud se datové praktiky aplikace v budoucí verzi změní, tyto zásady budou odpovídajícím způsobem aktualizovány.</p>

  <h2>Kontakt</h2>
  <p>Pokud máš jakékoli dotazy k těmto zásadám, napiš nám na: <a href="mailto:office@bookdragondev.com">office@bookdragondev.com</a></p>

  <p style="margin-top:32px"><a href="zoncna.html">&larr; Zpět na ZONCNA</a></p>

</div>

<footer>
  <span>&copy;&nbsp;2025&nbsp;Bookdragon Development</span>
  <a href="zoncna-privacy-policy.html">Privacy policy</a>
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

- [ ] **Step 2: Verify structure matches the QuestDeck privacy policy pattern**

Run:
```bash
grep -c "<h2>" zoncna-privacy-policy.html
grep -c "<h2>" questdeck-privacy-policy.html
```
Expected: both return `7` (same section count: Sběr dat/Data Collection, Data uložená.../Data Stored, Síťový přístup/Network Access, Služby třetích stran/Third-Party, Ochrana dětí/Children's, Změny/Changes, Kontakt/Contact).

- [ ] **Step 3: Commit**

```bash
git add zoncna-privacy-policy.html
git commit -m "feat: add ZONCNA privacy policy page"
```

---

### Task 5: Add "Zoncna" nav link to the six existing pages

**Files:**
- Modify: `index.html:44`
- Modify: `agility-game.html:66`
- Modify: `questdeck.html:59`
- Modify: `contact.html:29`
- Modify: `privacy-policy.html:28`
- Modify: `questdeck-privacy-policy.html:28`

**Interfaces:**
- Consumes: `zoncna.html` must already exist (Task 3) so the new nav link doesn't 404
- Produces: every page on the site now links to `zoncna.html` in its nav — later tasks and the Task 9 link-check depend on this being applied to all six files

- [ ] **Step 1: Update `index.html`**

Find (line 43-45):
```html
        <li><a href="agility-game.html">Agility Game</a></li>
        <li><a href="questdeck.html">QuestDeck</a></li>
        <li><a href="contact.html">Contact</a></li>
```
Replace with:
```html
        <li><a href="agility-game.html">Agility Game</a></li>
        <li><a href="questdeck.html">QuestDeck</a></li>
        <li><a href="zoncna.html">Zoncna</a></li>
        <li><a href="contact.html">Contact</a></li>
```

- [ ] **Step 2: Update `agility-game.html`**

Find (line 64-67):
```html
      <ul id="nav-menu">
        <li><a href="agility-game.html" class="active">Agility Game</a></li>
        <li><a href="questdeck.html">QuestDeck</a></li>
        <li><a href="contact.html">Contact</a></li>
```
Replace with:
```html
      <ul id="nav-menu">
        <li><a href="agility-game.html" class="active">Agility Game</a></li>
        <li><a href="questdeck.html">QuestDeck</a></li>
        <li><a href="zoncna.html">Zoncna</a></li>
        <li><a href="contact.html">Contact</a></li>
```

- [ ] **Step 3: Update `questdeck.html`**

Find (line 57-60):
```html
      <ul id="nav-menu">
        <li><a href="agility-game.html">Agility Game</a></li>
        <li><a href="questdeck.html" class="active">QuestDeck</a></li>
        <li><a href="contact.html">Contact</a></li>
```
Replace with:
```html
      <ul id="nav-menu">
        <li><a href="agility-game.html">Agility Game</a></li>
        <li><a href="questdeck.html" class="active">QuestDeck</a></li>
        <li><a href="zoncna.html">Zoncna</a></li>
        <li><a href="contact.html">Contact</a></li>
```

- [ ] **Step 4: Update `contact.html`**

Find (line 27-30):
```html
      <ul id="nav-menu">
        <li><a href="agility-game.html">Agility Game</a></li>
        <li><a href="questdeck.html">QuestDeck</a></li>
        <li><a href="contact.html" class="active">Contact</a></li>
```
Replace with:
```html
      <ul id="nav-menu">
        <li><a href="agility-game.html">Agility Game</a></li>
        <li><a href="questdeck.html">QuestDeck</a></li>
        <li><a href="zoncna.html">Zoncna</a></li>
        <li><a href="contact.html" class="active">Contact</a></li>
```

- [ ] **Step 5: Update `privacy-policy.html`**

Find (line 26-29):
```html
      <ul id="nav-menu">
        <li><a href="agility-game.html" class="active">Agility Game</a></li>
        <li><a href="questdeck.html">QuestDeck</a></li>
        <li><a href="contact.html">Contact</a></li>
```
Replace with:
```html
      <ul id="nav-menu">
        <li><a href="agility-game.html" class="active">Agility Game</a></li>
        <li><a href="questdeck.html">QuestDeck</a></li>
        <li><a href="zoncna.html">Zoncna</a></li>
        <li><a href="contact.html">Contact</a></li>
```

- [ ] **Step 6: Update `questdeck-privacy-policy.html`**

Find (line 26-29):
```html
      <ul id="nav-menu">
        <li><a href="agility-game.html">Agility Game</a></li>
        <li><a href="questdeck.html" class="active">QuestDeck</a></li>
        <li><a href="contact.html">Contact</a></li>
```
Replace with:
```html
      <ul id="nav-menu">
        <li><a href="agility-game.html">Agility Game</a></li>
        <li><a href="questdeck.html" class="active">QuestDeck</a></li>
        <li><a href="zoncna.html">Zoncna</a></li>
        <li><a href="contact.html">Contact</a></li>
```

- [ ] **Step 7: Verify all eight HTML pages now link to zoncna.html**

Run: `grep -l 'href="zoncna.html"' *.html | sort`
Expected: exactly 8 files listed — `agility-game.html`, `contact.html`, `index.html`, `privacy-policy.html`, `questdeck-privacy-policy.html`, `questdeck.html`, `zoncna-privacy-policy.html`, `zoncna.html`.

- [ ] **Step 8: Commit**

```bash
git add index.html agility-game.html questdeck.html contact.html privacy-policy.html questdeck-privacy-policy.html
git commit -m "feat: add Zoncna to site-wide navigation"
```

---

### Task 6: Add Zoncna product card to the homepage

**Files:**
- Modify: `index.html:77` (section heading text)
- Modify: `index.html:124-126` (insert third `<article class="hpcard">` after the QuestDeck card)
- Modify: `style.css:967` (`.home-product-grid` column count)

**Interfaces:**
- Consumes: `images/zoncna-icon.png`, `images/zoncna-FeaturedGraphic.png` (Task 1); `--accent-zn` and `.hpcard__cta--zn` (Task 2); `zoncna.html` (Task 3)
- Produces: homepage now advertises three products — no later task depends on this, it's the final content task before sitemap/redirects

- [ ] **Step 1: Update the section heading to reflect three products**

Find (`index.html` line 76-78):
```html
    <p class="home-sec-label">What we've built</p>
    <h2 class="home-sec-title">Two apps. One standard of care.</h2>
    <p class="home-sec-sub">Very different experiences — same commitment to making something genuinely good.</p>
```
Replace with:
```html
    <p class="home-sec-label">What we've built</p>
    <h2 class="home-sec-title">Three apps. One standard of care.</h2>
    <p class="home-sec-sub">Very different experiences — same commitment to making something genuinely good.</p>
```

- [ ] **Step 2: Insert the third product card**

Find (`index.html` line 119-127):
```html
        <p class="hpcard__desc">Feeling stuck indoors? QuestDeck is a pocket deck of real-world activity ideas. Pick a mood, flip three cards, go do something real. No account. Works offline. Always.</p>
        <div class="hpcard__stores">
          <span class="badge">Google Play</span>
          <span class="badge badge--alpha">Coming soon</span>
        </div>
        <a class="hpcard__cta hpcard__cta--qd" href="questdeck.html">Join the alpha &rarr;</a>
      </div>
    </article>

  </div>
</section>
```
Replace with:
```html
        <p class="hpcard__desc">Feeling stuck indoors? QuestDeck is a pocket deck of real-world activity ideas. Pick a mood, flip three cards, go do something real. No account. Works offline. Always.</p>
        <div class="hpcard__stores">
          <span class="badge">Google Play</span>
          <span class="badge badge--alpha">Coming soon</span>
        </div>
        <a class="hpcard__cta hpcard__cta--qd" href="questdeck.html">Join the alpha &rarr;</a>
      </div>
    </article>

    <article class="hpcard">
      <div class="hpcard__shot">
        <img src="images/zoncna-FeaturedGraphic.png" alt="Zoncna — radar map of Brno with rain clouds closing in" loading="lazy" />
        <div class="hpcard__shot-overlay"></div>
      </div>
      <div class="hpcard__body">
        <div class="hpcard__header">
          <img class="hpcard__icon" src="images/zoncna-icon.png" alt="Zoncna icon" width="52" height="52" />
          <div>
            <div class="hpcard__title">Zoncna</div>
            <div class="hpcard__platform">Browser &middot; Android &middot; <span style="color:var(--accent-zn)">Beta</span></div>
          </div>
        </div>
        <p class="hpcard__desc">An ironic weather-radar game where you drag the city of Brno around a map to dodge rain. Deadpan Czech humor, endless survival scoring, local high scores only. Czech-language only.</p>
        <div class="hpcard__stores">
          <span class="badge badge--alpha">Closed beta</span>
        </div>
        <a class="hpcard__cta hpcard__cta--zn" href="zoncna.html">Join the beta &rarr;</a>
      </div>
    </article>

  </div>
</section>
```

- [ ] **Step 3: Change the product grid from 2 to 3 columns**

Find (`style.css` line 965-971):
```css
.home-product-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  max-width: var(--max-w);
  margin: 0 auto;
  padding: 0 24px;
```
Replace with:
```css
.home-product-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
  max-width: var(--max-w);
  margin: 0 auto;
  padding: 0 24px;
```

The existing `@media (max-width: 768px) { .home-product-grid { grid-template-columns: 1fr; ... } }` rule (style.css line ~1138) already collapses to a single column on mobile/tablet — no change needed there.

- [ ] **Step 4: Verify the homepage has three cards and the grid is 3-column**

Run:
```bash
grep -c "<article class=\"hpcard\">" index.html
grep "grid-template-columns: repeat(3, 1fr);" style.css
```
Expected: first command prints `3`; second command prints the matching line (confirms the grid rule was updated, not just the QuestDeck-adjacent duplicate).

- [ ] **Step 5: Commit**

```bash
git add index.html style.css
git commit -m "feat: add Zoncna product card to homepage"
```

---

### Task 7: Add Zoncna URLs to `sitemap.xml`

**Files:**
- Modify: `sitemap.xml:32` (insert two new `<url>` entries after the QuestDeck privacy policy entry)

**Interfaces:**
- Consumes: `zoncna.html`, `zoncna-privacy-policy.html` (Tasks 3, 4) must exist at the canonical URLs referenced
- Produces: nothing consumed by later tasks — this is a leaf/informational file

- [ ] **Step 1: Insert the two new entries**

Find (`sitemap.xml` line 27-32):
```xml
  <url>
    <loc>https://www.bookdragondev.com/questdeck-privacy-policy.html</loc>
    <lastmod>2026-05-15</lastmod>
    <changefreq>yearly</changefreq>
    <priority>0.3</priority>
  </url>
```
Replace with:
```xml
  <url>
    <loc>https://www.bookdragondev.com/questdeck-privacy-policy.html</loc>
    <lastmod>2026-05-15</lastmod>
    <changefreq>yearly</changefreq>
    <priority>0.3</priority>
  </url>
  <url>
    <loc>https://www.bookdragondev.com/zoncna.html</loc>
    <lastmod>2026-07-16</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.9</priority>
  </url>
  <url>
    <loc>https://www.bookdragondev.com/zoncna-privacy-policy.html</loc>
    <lastmod>2026-07-16</lastmod>
    <changefreq>yearly</changefreq>
    <priority>0.3</priority>
  </url>
```

- [ ] **Step 2: Verify well-formed XML with the right entry count**

Run:
```bash
grep -c "<url>" sitemap.xml
grep -c "</url>" sitemap.xml
grep "zoncna" sitemap.xml
```
Expected: both counts are `8`; the third command shows both new `<loc>` lines.

- [ ] **Step 3: Commit**

```bash
git add sitemap.xml
git commit -m "feat: add Zoncna URLs to sitemap"
```

---

### Task 8: Add `/zoncna` redirect to `netlify.toml`

**Files:**
- Modify: `netlify.toml:40` (insert new redirect block before `[build]`)

**Interfaces:**
- Consumes: `zoncna.html` (Task 3)
- Produces: nothing consumed by later tasks

- [ ] **Step 1: Insert the redirect**

Find (`netlify.toml` line 36-42):
```toml
[[redirects]]
  from = "/questdeck-privacy-policy"
  to = "/questdeck-privacy-policy.html"
  status = 301

[build]
  publish = "."
```
Replace with:
```toml
[[redirects]]
  from = "/questdeck-privacy-policy"
  to = "/questdeck-privacy-policy.html"
  status = 301

[[redirects]]
  from = "/zoncna"
  to = "/zoncna.html"
  status = 301

[build]
  publish = "."
```

- [ ] **Step 2: Verify the redirect block was added and `[build]` still comes last**

Run: `grep -A2 'from = "/zoncna"' netlify.toml`
Expected:
```
  from = "/zoncna"
  to = "/zoncna.html"
  status = 301
```

- [ ] **Step 3: Commit**

```bash
git add netlify.toml
git commit -m "feat: add /zoncna redirect"
```

---

### Task 9: Full-site link and image integrity check

**Files:** none created or modified — verification only.

**Interfaces:**
- Consumes: every file touched in Tasks 1-8
- Produces: nothing — this is the final gate before considering the feature complete

- [ ] **Step 1: Scan every HTML file for local links/images that don't resolve to a real file**

Run:
```bash
for f in *.html; do
  grep -oE '(href|src)="[^"]+"' "$f" | sed -E 's/^(href|src)="//; s/"$//' | while read -r path; do
    case "$path" in
      http*|mailto:*|\#*) continue ;;
    esac
    if [ ! -f "$path" ]; then
      echo "BROKEN in $f: $path"
    fi
  done
done
```
Expected: no output (every local `href`/`src` resolves to a file that exists in the repo).

- [ ] **Step 2: Confirm no leftover placeholder text**

Run: `grep -rniE "TBD|TODO|lorem ipsum|coming soon\?" zoncna.html zoncna-privacy-policy.html`
Expected: no matches (exit code 1 / empty output).

- [ ] **Step 3: Manual browser smoke test**

Run a local static server:
```bash
npx serve . -l 5000
```
Then, in a browser, visit each of the following and confirm visually:
- `http://localhost:5000/zoncna.html` — dark navy theme renders, hero image loads, hamburger menu works on a narrow window, both screenshots load, CTA buttons point to the Google Form
- `http://localhost:5000/zoncna-privacy-policy.html` — matches the ZONCNA theme, highlight box renders, back-link returns to `zoncna.html`
- `http://localhost:5000/index.html` — three cards render side by side on desktop width, one column on a narrow window, no layout overlap
- `http://localhost:5000/agility-game.html`, `/questdeck.html`, `/contact.html` — nav bar shows "Zoncna" between the other app and "Contact", clicking it lands on `zoncna.html`

Stop the server (Ctrl+C) once confirmed.

- [ ] **Step 4: No commit for this task** (verification only — if Step 1 or 2 finds anything, fix it in the relevant earlier task's files and re-run before proceeding)
