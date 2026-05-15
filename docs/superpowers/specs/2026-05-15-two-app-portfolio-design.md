# Design: Two-App Portfolio Website

**Date:** 2026-05-15
**Scope:** Restructure www.bookdragondev.com from a single-product Agility Game site to a two-product Bookdragon Development portfolio featuring Agility Game and QuestDeck.

---

## Architecture

**Chosen approach: Symmetric portfolio (Option C)**  
Homepage is a company rozcestník. Each app has its own dedicated landing page. Both apps are equal in navigation weight.

### File structure (after)

| File | Role |
|---|---|
| `index.html` | New Bookdragon Development homepage — two product cards |
| `agility-game.html` | Current `index.html` content, renamed |
| `questdeck.html` | New QuestDeck landing page |
| `privacy-policy.html` | Agility Game privacy policy (existing, nav updated) |
| `questdeck-privacy-policy.html` | QuestDeck privacy policy (new, exact text from `docs/store/privacy-policy.md`) |
| `contact.html` | Existing, nav updated only |
| `sitemap.xml` | Updated with new URLs |
| `netlify.toml` | New redirects added |

---

## Navigation (all pages)

```
[Bookdragon logo]   Agility Game | QuestDeck | Contact
```

- Logo/name links to `index.html`
- Active state on current page
- Hamburger menu on mobile (same JS as today, duplicated per page)

---

## index.html — Bookdragon Homepage

**Hero:**
- Small label: "Bookdragon Development"
- Headline: "We make games and apps worth your time."
- Subtext: one line about both products

**Product cards (2-column grid, 1-column on mobile):**

_Agility Game card_
- Accent bar: `#e08053`
- Icon: Bookdragon logo (`images/logo.png`)
- Title, platform badges (Epic Store · Steam · Google Play)
- Short description (1–2 sentences)
- CTA: "More info →" → `agility-game.html`

_QuestDeck card_
- Accent bar: `#FF8C42`
- Icon: `assets/QD_ikon.png` (copied to `images/questdeck-icon.png`)
- Title, platform badge (Google Play), "Alpha" badge
- Short description (1–2 sentences)
- CTA: "Join alpha →" → `questdeck.html`

**Footer:** © 2025 Všechna práva vyhrazena | Contact

---

## agility-game.html

Current `index.html` content, with two changes:
1. Navigation updated to new structure
2. Footer updated: `privacy-policy.html` link stays, contact stays

No content changes.

---

## questdeck.html — QuestDeck Landing Page

**Hero (2-column):**
- Left: "Alpha" badge, title "QuestDeck — Real-Life Activity Cards", tagline, "Join alpha testing →" button (mailto:office@bookdragondev.com), email address shown
- Right: App icon (`images/questdeck-icon.png`)

**How it works (3-column):**
- Pick a mood → Flip 3 cards → Go do it

**Key features (2-column checklist):**
- 100+ real-world quests — solo, couples, friends
- Works fully offline — no internet required
- No account, no login, no tracking
- XP system, levels, and progress history
- Easy, medium, and hard difficulty
- Android — Google Play (coming soon)

**Alpha CTA section:**
- Headline: "Want to be one of the first?"
- Body: QuestDeck is in alpha testing. Write to us and get access.
- Button: mailto:office@bookdragondev.com

**Footer:** © 2025 | QuestDeck Privacy Policy → `questdeck-privacy-policy.html` | Contact

---

## questdeck-privacy-policy.html — QuestDeck Privacy Policy

**Source:** exact text from `C:\Projects\AI\Claude\QuestDeck\docs\store\privacy-policy.md`

**Layout:** same `.page-header` / `.page-content` structure as `privacy-policy.html`

**Differences from Agility Game privacy policy page:**
- Accent color `#FF8C42` used in header app label and links
- Highlighted "no data" box at top (`.page-content` leading callout)
- "Last updated: May 15, 2026" subtitle in page header
- "Back to QuestDeck" link at bottom → `questdeck.html`

**Content sections (exact match to MD):**
1. Data Collection
2. Data Stored on Your Device
3. Network Access
4. Third-Party Services
5. Children's Privacy
6. Changes to This Policy
7. Contact — email: tomas.hutyra@gmail.com (exact as per privacy-policy.md)

---

## CSS changes (style.css)

- Add `--accent-qd: #FF8C42;` and `--accent-qd-h: #e07a32;` CSS variables
- Add `.product-cards` grid layout (homepage cards)
- Add `.product-card` component with accent bar variant via `data-accent`
- Add `.alpha-badge` inline badge
- Add `.highlight-box` callout for QuestDeck privacy policy
- Add `.btn-alpha` button style (uses `--accent-qd`)
- Existing styles unchanged

---

## netlify.toml additions

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

---

## sitemap.xml

Add entries for:
- `/agility-game.html`
- `/questdeck.html`
- `/questdeck-privacy-policy.html`

Update `index.html` entry (now homepage, not Agility Game landing).

---

## Out of scope

- Screenshots for QuestDeck (app is in alpha — no screenshots yet)
- Google Play link for QuestDeck (not live yet — add when published)
- Agility Game content changes
- Czech vs English language decisions (keep existing mix)
