# Design: Zoncna Landing Page + Privacy Policy

**Date:** 2026-07-16
**Scope:** Add ZONCNA as a third product to the Bookdragon Development portfolio site: a new landing page, a new privacy policy page, and integration into the shared nav/homepage/sitemap. Source project: `c:\Projects\AI\Claude\Zoncna\`.

---

## Product background

ZONCNA ("simulátor brněnských srážek") is an ironic Czech-language arcade game — a parody weather-radar app in which the player drags the city of Brno around a static map of the Brno area to dodge rain clouds. Deadpan meteorological tone, Brno hantec references. Endless survival loop, local top-10 high-score table ("kronika sucha") stored in `localStorage`, no accounts, no ads, no analytics, no network requests at runtime (per `Doc/ZONCNA-GDD.md`). Ships as a browser build and an Android wrapper (Capacitor). Not yet publicly live — currently recruiting closed testers via a Google Form.

**Language decision:** the ZONCNA landing page and its privacy policy are written in **Czech** (matches the game's own voice). Shared site chrome — header nav labels, footer links — stays in **English**, consistent with every other page on the site. Only the page's own content (hero, section copy, CTA) is Czech.

**CTA target:** `https://forms.gle/8CmkTbt7TWKeRiad7` (closed-testing opt-in form), used in place of the `mailto:` pattern QuestDeck uses.

---

## File structure (after)

| File | Role |
|---|---|
| `zoncna.html` | New ZONCNA landing page (`body.zn-page`) |
| `zoncna-privacy-policy.html` | New ZONCNA privacy policy (`body.zn-page`) |
| `index.html` | Add third product card + nav link |
| `agility-game.html`, `questdeck.html`, `contact.html`, `privacy-policy.html`, `questdeck-privacy-policy.html` | Nav updated only (add "Zoncna" link) |
| `style.css` | New `body.zn-page` theme block + `zn-*` section classes |
| `sitemap.xml` | Add `zoncna.html`, `zoncna-privacy-policy.html` |
| `netlify.toml` | Add `/zoncna` → `/zoncna.html` redirect |
| `images/zoncna-icon.png` | Copied from `Zoncna/mobile/resources/icon.png` |
| `images/zoncna-FeaturedGraphic.png` | Copied from `Zoncna/Doc/FeatureGraphic1024x500.png` |
| `images/zoncna-screenshot-1.jpg`, `images/zoncna-screenshot-2.jpg` | Copied from `Zoncna/Doc/Screenshot_20260716_071154_ZONCNA.jpg` (start screen) and `Screenshot_20260716_071309_ZONCNA.jpg` (gameplay) |

---

## Navigation (all pages)

```
[Bookdragon logo]   Agility Game | QuestDeck | Zoncna | Contact
```

Same hamburger JS snippet, kept in sync across all HTML files per existing convention.

---

## Visual theme — `body.zn-page` (style.css)

Colors are taken directly from the game's actual in-app UI (confirmed via screenshots), not invented fresh — cooler/darker than `ag-page` so the two dark-themed products don't read as the same product:

```css
body.zn-page {
  --bg-dark:   #11172a;   /* navy-slate, cooler than ag-page's #0d0a05 */
  --bg-dark2:  #0b0f1d;
  --bg-alt:    #1a2338;
  --accent:    #f0b429;   /* amber-gold, matches in-game CTA/title-rule color */
  --accent-h:  #d69a1a;
  --accent-zn-blue: #3b8bd6;  /* secondary radar-blue, from GPS pin / feature graphic */
}
```

Section classes mirror the `qd-page` "-v2" pattern under new `zn-` names (`.zn-hero`, `.zn-hero__title`, `.zn-section-header`, `.zn-steps`, `.zn-step`, `.zn-features`, `.zn-feat`, `.zn-cta`), scoped generically (not `body.zn-page .zn-hero` — same approach questdeck uses of unprefixed component classes since only one page uses them). Nav hover/active and `.page-header`/`.page-content` privacy-policy overrides added under `body.zn-page` following the existing `ag-page`/`qd-page` blocks at the bottom of `style.css`.

---

## zoncna.html — ZONCNA Landing Page

**Hero (2-column, image right):**
- Label: "Beta test · Prohlížeč + Android"
- Title: "Táhni prstem. Uhni s celým městem." (lifted from the game's own start-screen copy for authenticity)
- Subtext: ZONCNA je ironický radar-simulátor brněnských srážek. Neovládáš mrak, ale celé město — cílem je udržet Brno v suchu, zatímco se od okrajů mapy valí barevné peklo. Čím dél vydržíš suchý, tím vyšší suchoskóre.
- CTA: "Přihlásit se do closed testu →" → `https://forms.gle/8CmkTbt7TWKeRiad7`
- Small note under CTA: "nebo napiš na office@bookdragondev.com"
- Image: `images/zoncna-FeaturedGraphic.png`

**How it works (3-step, mirrors qd-page steps layout):**
1. Táhni Brno po radaru — uhýbej srážkám tažením prstu nebo myši.
2. Sleduj promočení — čím silnější odraz zasáhne město, tím rychleji roste; mimo déšť pomalu vysychá.
3. Zapiš se do kroniky sucha — v top 10 zadáš jméno a poměříš se s ostatními suchaři.

**Features grid (checkmark list, mirrors qd-page features):**
- Skutečný mapový podklad Brna a okolí (~30 km)
- Endless mode s plynule rostoucí obtížností
- Lokální kronika sucha (top 10) — žádný účet potřeba
- Nulový sběr dat, nulové reklamy
- Ironický, suchý meteorologický tón — brněnský hantec vítán
- Prohlížeč (PC i mobil) + Android (beta)

**Screenshots strip (2-up, mirrors ag-page dogs-grid pattern at smaller scale):**
- `images/zoncna-screenshot-1.jpg` (start screen)
- `images/zoncna-screenshot-2.jpg` (gameplay/radar view)

**CTA section:**
- Headline: "Chceš být první suchař?"
- Body: ZONCNA sbírá lidi do closed testu. Přihlas se přes formulář a pomoz nám hru doladit.
- Button: "Vyplnit formulář →" → `https://forms.gle/8CmkTbt7TWKeRiad7`

**Attribution line (small print caption directly under the hero image):**
"Mapové podklady: © přispěvatelé OpenStreetMap, © CARTO" — required attribution for the bundled static map extract (per `Doc/ZONCNA-GDD.md` map data source), not a data-collection disclosure.

**Footer:** same shared footer pattern → `zoncna-privacy-policy.html` | `contact.html`

**SEO:** full OG/Twitter/Schema.org (`MobileApplication`, `applicationCategory: "GameApplication"`, `operatingSystem: "Web, Android"`) in Czech, canonical `https://www.bookdragondev.com/zoncna.html`.

---

## zoncna-privacy-policy.html — ZONCNA Privacy Policy

Same `.page-header` / `.page-content` structure as `questdeck-privacy-policy.html`, in Czech.

**Highlight box:**
- Title: "ZONCNA neshromažďuje žádná osobní data."
- Sub: "Hra funguje offline a data z tvého zařízení nikam neodesílá."

**Sections:**
1. Sběr dat — žádný účet, žádná registrace, žádný sběr osobních údajů.
2. Data uložená v zařízení — kronika sucha (jméno + skóre, `localStorage`), odemčené odznaky, nastavení zvuku. Nikdy neopouští zařízení.
3. Síťový přístup — hra funguje offline; mapový podklad je statický a součástí instalace/buildu.
4. Služby třetích stran — žádné SDK, reklamy, analytika ani crash reporting.
5. Ochrana dětí — aplikace nesbírá data od nikoho, včetně dětí do 13 let.
6. Změny zásad — pokud se datové praktiky v budoucí verzi změní, zásady budou aktualizovány.
7. Kontakt — office@bookdragondev.com
- Back-link: "← Zpět na ZONCNA" → `zoncna.html`

---

## index.html — homepage integration

Add third `.hpcard` (English, consistent with existing two cards):
- Accent: `--accent-zn-blue` for icon/CTA bar (keeps homepage card row visually distinct from the gold Agility Game card)
- Icon: `images/zoncna-icon.png`
- Title: "Zoncna", platform line: "Browser · Android · Beta", small note "Czech only"
- Description (English, 1–2 sentences): "An ironic weather-radar game where you drag the city of Brno around a map to dodge rain. Deadpan Czech humor, endless survival scoring, local high scores only."
- CTA: "Join the beta →" → `zoncna.html`

Nav updated to include "Zoncna" between "QuestDeck" and "Contact".

New homepage-only CSS: `.hpcard__cta--zn` (background `var(--accent-zn-blue)`, hover a darkened variant), following the exact pattern of the existing `.hpcard__cta--ag` / `.hpcard__cta--qd` rules.

---

## netlify.toml addition

```toml
[[redirects]]
  from = "/zoncna"
  to = "/zoncna.html"
  status = 301
```

---

## sitemap.xml

Add entries for `/zoncna.html` and `/zoncna-privacy-policy.html`, following the existing entry format.

---

## Out of scope

- Live web build embed/iframe (game isn't public yet — CTA links out to the signup form instead)
- Google Play store badge/link (not published yet)
- Translating shared header/footer chrome to Czech
- Changes to the Zoncna game project itself (`c:\Projects\AI\Claude\Zoncna\`) — this spec only covers the marketing site
