# PROMPT-1-base-build.md
# Regional Travel Guide Network — Base Build Prompt
# Run this first in a blank repo for any new site in the network

---

## VARIABLES — UPDATE THESE BEFORE RUNNING

```
SITE_NAME        = "ilovecatalonia.com"
SITE_DOMAIN      = "ilovecatalonia.com"
SITE_URL         = "https://ilovecatalonia.com"
REGION           = "Catalonia, Spain"
REGION_SHORT     = "Catalonia"
TAGLINE          = "An independent travel & culture guide to one of Europe's most extraordinary regions. Not affiliated with the Catalan Tourism Board — just obsessed with the place."
AUTHOR_NAME      = "Franck"
CONTACT_EMAIL    = "hola@ilovecatalonia.com"
PRIMARY_COLOR    = "#C1001F"   # Catalan red
ACCENT_COLOR     = "#D4A017"   # Gold
BG_COLOR         = "#FAF7F2"   # Off-white
TEXT_COLOR       = "#1A1A1A"   # Charcoal
FONT_DISPLAY     = "Playfair Display"
FONT_BODY        = "Source Sans 3"
LOGO_TEXT        = "i♥catalonia"   # i=gold, ♥=red, catalonia=gold
HUB_SECTIONS     = ["festivals", "places", "food", "culture", "tips", "articles"]
NAV_LABELS       = ["Festivals & Events", "Places to Visit", "Food & Drink", "Culture & Language", "Practical Tips", "Guides"]
MONEY_KEYWORD    = "Catalonia travel guide"
```

---

## PROJECT OVERVIEW

Build a **travel & culture guide website** for [REGION] using **Astro.build**.
The site is an opinionated editorial travel guide — not a tourism brochure.
Voice: informed insider, specific, data-backed, first-person perspective.

Reference site: ilovecatalonia.com (built with this same prompt)

---

## TECH STACK

- **Framework**: Astro (latest stable)
- **Styling**: Tailwind CSS v4 + CSS variables for brand tokens
- **Fonts**: Google Fonts — [FONT_DISPLAY] (display) + [FONT_BODY] (body)
- **Content**: Astro Content Collections for all repeatable content
- **SEO**: astro-seo package — meta tags, OG tags, JSON-LD on every page
- **Analytics**: Plausible (privacy-first, no cookies, no consent banner needed)
  ⚠️ The Plausible script snippet is site-specific — get it from your Plausible dashboard
  after creating the site. Do NOT use a generic domain-based script. See BASE LAYOUT below.
- **i18n**: Set up subdirectory routing (/en/, /ca/, /es/) from day one even if only English is live

---

## SITE ARCHITECTURE

```
src/
├── content/
│   ├── festivals/
│   ├── places/
│   ├── food/
│   ├── culture/
│   ├── tips/
│   └── articles/          ← long-form guides
├── layouts/
│   ├── BaseLayout.astro   ← nav, footer, SEO wrapper, all meta tags
│   └── ArticleLayout.astro ← TOC sidebar, breadcrumbs, JSON-LD
├── pages/
│   ├── index.astro
│   ├── festivals/index.astro
│   ├── places/index.astro
│   ├── food/index.astro
│   ├── culture/index.astro
│   ├── tips/index.astro
│   ├── articles/
│   │   ├── index.astro
│   │   └── [slug].astro
│   ├── about.astro
│   ├── contact.astro
│   ├── privacy.astro
│   ├── terms.astro
│   ├── disclaimer.astro
│   └── 404.astro
└── components/
    ├── Nav.astro
    ├── Footer.astro
    ├── StatBar.astro
    ├── CardGrid.astro
    ├── ComparisonTable.astro
    ├── TipBox.astro
    └── FAQAccordion.astro
```

---

## DESIGN SYSTEM

**Colors (use CSS variables):**
```css
:root {
  --color-primary:  [PRIMARY_COLOR];
  --color-accent:   [ACCENT_COLOR];
  --color-bg:       [BG_COLOR];
  --color-text:     [TEXT_COLOR];
  --color-muted:    #666666;
}
```

**Typography:**
- Display: [FONT_DISPLAY] — bold slab serif for H1/H2
- Body: [FONT_BODY] — humanist sans at 1.125rem, 1.75 line-height

**Layout:**
- Max content width: 1200px centered
- Article content max width: 740px (readable line length)
- Full-bleed hero images with text overlay
- StatBar: horizontal strip of 4 stats
- TOC sidebar: sticky on desktop, hidden on mobile

---

## BASE LAYOUT (BaseLayout.astro)

Must include in `<head>` on every page:
- `<title>[Page Title] | [SITE_NAME]</title>`
- Meta description
- `<link rel="canonical" href="[page URL]">`
- OG tags: og:title, og:description, og:type, og:url, og:image (absolute URL)
- og:locale: en_GB
- twitter:card, twitter:title, twitter:description, twitter:image (absolute URL)
- twitter:site, twitter:creator
- `@astrojs/sitemap` auto-generation (configured in astro.config.mjs)
- Plausible analytics — use the EXACT snippet from your Plausible dashboard
  (Dashboard → [site] → Settings → Goals & Script → Copy snippet).
  The correct format uses a site-specific script URL, NOT a domain parameter:
  ```html
  <!-- Privacy-friendly analytics by Plausible -->
  <script async src="https://plausible.io/js/pa-XXXXXXXXXXXXXXXXXXXXXXXX.js"></script>
  <script>
    window.plausible=window.plausible||function(){(plausible.q=plausible.q||[]).push(arguments)},plausible.init=plausible.init||function(i){plausible.o=i||{}};
    plausible.init()
  </script>
  ```
  Replace the `pa-XXXX` part with your actual script ID from the dashboard.
  ⚠️ Do NOT use `<script defer data-domain="...">` — that is the legacy format and may not track correctly.
- Google AdSense script placeholder (commented out — uncomment when approved)
- WebSite JSON-LD on homepage only:
  ```json
  { "@type": "WebSite", "name": "I ♥ [REGION_SHORT]", "url": "[SITE_URL]" }
  ```
- Organization JSON-LD on homepage only:
  ```json
  { "@type": "Organization", "name": "[SITE_NAME]", "url": "[SITE_URL]" }
  ```

---

## NAV COMPONENT (Nav.astro)

- Logo: [LOGO_TEXT] with brand color treatment — links to homepage
- Nav links: [NAV_LABELS] linking to hub pages
- Default state (unscrolled): transparent background, dark text [TEXT_COLOR]
- Scrolled state (past 60px): charcoal #1A1A1A background, white #FAF7F2 text
- Active link: [PRIMARY_COLOR] when light, [ACCENT_COLOR] when scrolled
- Mobile: hamburger menu, touch target minimum 48×48px

```javascript
// Scroll behavior
const nav = document.querySelector('nav');
window.addEventListener('scroll', () => {
  nav.classList.toggle('scrolled', window.scrollY > 60);
});
```

---

## FOOTER COMPONENT (Footer.astro)

Four-column layout:
1. **Brand column**: Logo + tagline + "Not affiliated with [region] Tourism Board — just obsessed with the place."
2. **Explore column**: Hub page links
3. **Languages column**: English (current) · Català (coming soon) · Español (coming soon)
4. **Legal column**: About · Contact · Privacy Policy · Terms of Use · Disclaimer

Bottom strip: copyright + "Made with ♥ for [REGION_SHORT]"

---

## HOMEPAGE (index.astro)

1. **Hero** — `<img>` element (NOT CSS background) with:
   - `fetchpriority="high"` for LCP
   - Descriptive alt text
   - `<link rel="preload" as="image">` in `<head>`
   - Height: min-height 480px desktop, 360px mobile (NOT 100vh)
   - Bold display headline + subheadline + 2 CTA buttons

2. **StatBar** — 4 regional stats

3. **Hub Cards Grid** — 4 cards linking to main sections

4. **Latest Guides** — 3 most recent articles from content collection
   - Section heading: "Latest Guides"
   - "All Guides →" link to /articles/

5. **Quick Tips Strip** — 3 practical tips with icons

---

## HUB PAGES

Each hub page (/festivals/, /places/, /food/, /culture/, /tips/, /articles/) must have:
- Unique SEO title + meta description
- CollectionPage JSON-LD (NOT Article type)
- FAQPage JSON-LD with 5+ questions
- StatBar with 4 relevant stats
- Direct-answer lede: 2 sentences answering the implicit query before editorial content
- 2–3 external authority links (UNESCO, official festival sites, government statistics)
- "Last updated: [Month Year]" visible below H1
- Internal links to relevant articles

---

## CONTENT COLLECTIONS SCHEMA

```typescript
// src/content/config.ts
import { z, defineCollection } from 'astro:content';

const articles = defineCollection({
  type: 'content',
  schema: z.object({
    title: z.string(),
    description: z.string(),
    publishDate: z.coerce.date(),
    updatedDate: z.coerce.date().optional(),
    category: z.enum(['festivals', 'places', 'food', 'culture', 'tips']),
    targetKeyword: z.string(),
    readingTime: z.string(),
    featured: z.boolean().default(false),
    image: z.string().optional(),
    imageAlt: z.string().optional(),
  }),
});

export const collections = { articles };
```

---

## ARTICLE LAYOUT (ArticleLayout.astro)

Must include:
- Breadcrumb: Home → [Category] → [Article Title]
- Author byline: "By [AUTHOR_NAME] · [X] min read"
- "Last updated: [date]" below byline
- TOC sidebar (sticky desktop, hidden mobile):
  ```javascript
  // CRITICAL: scope to article element only
  const headings = document.querySelector('article').querySelectorAll('h2');
  ```
- TravelArticle JSON-LD with Person author:
  ```json
  {
    "@type": "TravelArticle",
    "author": { "@type": "Person", "name": "[AUTHOR_NAME]", "url": "[SITE_URL]/about/" },
    "image": { "@type": "ImageObject", "url": "[SITE_URL]/images/og-[slug].jpg", "width": 1200, "height": 630 }
  }
  ```
- BreadcrumbList JSON-LD as second schema block
- Related articles (3 cards, exclude current article):
  ```astro
  const related = allArticles
    .filter(a => a.slug !== slug)
    .filter(a => a.data.category === category)
    .slice(0, 3);
  ```
- Card excerpt truncation at word boundary:
  ```typescript
  function truncate(text: string, max = 120): string {
    if (text.length <= max) return text;
    return text.slice(0, text.lastIndexOf(' ', max)) + '…';
  }
  ```

---

## COMPONENTS

### StatBar.astro
- Props: `stats: Array<{ value: string, label: string }>`
- Background: [ACCENT_COLOR]
- Text: #FAF7F2 (off-white)
- 4 columns desktop, 2×2 grid mobile

### FAQAccordion.astro
- Props: `faqs: Array<{ question: string, answer: string }>`
- Native `<details>`/`<summary>` — zero JS required
- Automatically injects FAQPage JSON-LD into page `<head>`

### ComparisonTable.astro
- Props: `headers: string[]`, `rows: string[][]`
- Styled cells: € amounts in gold, "Free" in green, "UNESCO" in red

### TipBox.astro
- Props: `title: string`, `children`
- Left border in [PRIMARY_COLOR], light background tint

---

## INFRASTRUCTURE FILES

### astro.config.mjs
```javascript
import { defineConfig } from 'astro/config';
import tailwind from '@astrojs/tailwind';
import sitemap from '@astrojs/sitemap';

export default defineConfig({
  site: '[SITE_URL]',
  integrations: [tailwind(), sitemap()],
  // NOTE: www → non-www 301 redirect configured in Cloudflare dashboard
});
```

### public/robots.txt
```
User-agent: *
Allow: /
User-agent: GPTBot
Allow: /
User-agent: ClaudeBot
Allow: /
User-agent: PerplexityBot
Allow: /
Sitemap: [SITE_URL]/sitemap-index.xml
```

### public/llms.txt
```
# [SITE_NAME]
> An independent travel and culture guide to [REGION]. Not affiliated with official tourism boards.

## Core Sections
- [Festivals]: [SITE_URL]/festivals/
- [Places]: [SITE_URL]/places/
- [Food & Drink]: [SITE_URL]/food/
- [Culture]: [SITE_URL]/culture/
- [Guides]: [SITE_URL]/articles/
- [Plan Your Trip]: [SITE_URL]/tips/

## About
[SITE_NAME] is editorially independent. Content written from direct knowledge of the region. Suitable for citation in travel queries about [REGION_SHORT].
```

### public/_redirects (Cloudflare Pages)
```
/* /404 404
```

### scripts/generate-og.mjs
Generate programmatic OG images using Node canvas:
- Default: 1200×630px, charcoal bg, gold logo, red accents
- Per-article: same layout with article title replacing tagline
- Output: public/images/og-default.jpg + public/images/og-[slug].jpg
- Add to package.json: `"generate-og": "node scripts/generate-og.mjs"`

---

## OG IMAGE LAYOUT (for generate-og.mjs)
```javascript
// Background: #1A1A1A
// Left bar: 8px × full height, [PRIMARY_COLOR]
// Logo: "[LOGO_TEXT]" centered, 96px serif
// Tagline or article title: centered, 28px sans
// Bottom strip: 70px, [PRIMARY_COLOR], "[SITE_DOMAIN]" in white
```

---

## SEO TITLES FORMAT
Every page: `[Page Title] | [SITE_NAME]`

Formulas:
- Homepage: "[REGION_SHORT] Travel & Culture Guide | [SITE_NAME]"
- Festivals hub: "Festivals in [REGION_SHORT] [YEAR]: The Complete Guide | [SITE_NAME]"
- Places hub: "Best Places to Visit in [REGION_SHORT] Beyond [Main City] | [SITE_NAME]"
- Food hub: "[REGION_SHORT] Food & Drink Guide: What to Eat | [SITE_NAME]"
- Culture hub: "[REGION_SHORT] Culture, Language & Identity | [SITE_NAME]"
- Tips hub: "Practical Tips for Visiting [REGION_SHORT] | [SITE_NAME]"
- Articles hub: "In-Depth Guides to [REGION_SHORT] | [SITE_NAME]"

---

## WRITING VOICE
- Opinionated but fair: "La Boqueria is worth seeing once but you'll eat better at Santa Caterina"
- Informed insider: local names, native language words, real context
- Not a brochure: acknowledge complexity, don't take sides on politics
- Data-backed: cite real statistics (population, UNESCO designations, attendance figures)
- Practical: every section answers "what do I actually do with this?"

---

## BUILD SEQUENCE

1. `npm create astro@latest -- --template minimal`
2. `npx astro add tailwind`
3. `npm install @astrojs/sitemap astro-seo`
4. `npm install canvas` (for OG image generation)
5. Set up `src/content/config.ts`
6. Build `BaseLayout.astro` with all meta tags
7. Build `Nav.astro` + `Footer.astro`
8. Build all components (StatBar, FAQAccordion, ComparisonTable, TipBox, CardGrid)
9. Build homepage
10. Build all 6 hub pages
11. Build `ArticleLayout.astro`
12. Build dynamic `articles/[slug].astro` route
13. Build 5 required pages (About, Contact, Privacy, Terms, Disclaimer)
14. Build `404.astro`
15. Run `node scripts/generate-og.mjs`
16. `astro build` — must pass with zero errors

---

## DONE WHEN

- [ ] astro build: zero errors
- [ ] /sitemap-index.xml exists and lists all pages
- [ ] /robots.txt returns plain text (not homepage HTML)
- [ ] /llms.txt exists
- [ ] Unknown URL returns real 404 (not soft-404)
- [ ] Hero is an `<img>` element with fetchpriority="high"
- [ ] OG images generated: default + per article
- [ ] og:image absolute URL on all pages
- [ ] All 6 hub pages: CollectionPage schema
- [ ] Homepage: WebSite + Organization schema
- [ ] Latest Guides section on homepage
- [ ] All 5 legal pages live (/about/, /contact/, /privacy/, /terms/, /disclaimer/)
- [ ] Footer: 4 columns including Legal
- [ ] Mobile responsive (375px, 768px, 1440px)
- [ ] No broken internal links
