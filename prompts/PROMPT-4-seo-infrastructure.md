# PROMPT-4-seo-infrastructure.md
# Regional Travel Guide Network — SEO & Infrastructure Sprint
# Run after base build and UI fixes are complete.
# Covers: sitemap, robots, schema, OG images, site name, AdSense pages.

---

## VARIABLES — UPDATE BEFORE RUNNING

```
SITE_NAME        = "ilovecatalonia.com"
SITE_URL         = "https://ilovecatalonia.com"
SITE_DISPLAY_NAME = "I ♥ Catalonia"
REGION_SHORT     = "Catalonia"
AUTHOR_NAME      = "Franck"
CONTACT_EMAIL    = "hola@ilovecatalonia.com"
PRIMARY_COLOR    = "#C1001F"
ACCENT_COLOR     = "#D4A017"
LOGO_TEXT        = "i♥catalonia"
ADSENSE_CLIENT   = "ca-pub-XXXXXXXXXXXXXXXXX"   # Fill in after AdSense approval
```

---

## FIX 1 — SITEMAP + ROBOTS.TXT

Install if not present:
```bash
npm install @astrojs/sitemap
```

In astro.config.mjs:
```javascript
import sitemap from '@astrojs/sitemap';
export default defineConfig({
  site: '[SITE_URL]',
  integrations: [sitemap()],
});
```

Create public/robots.txt:
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

---

## FIX 2 — 404 PAGE + SOFT-404 FIX

Create public/_redirects:
```
/* /404 404
```

Create src/pages/404.astro with branded "Page not found" layout.
Include: site nav, link back to homepage, link to main sections.

---

## FIX 3 — LLMS.TXT

Create public/llms.txt:
```
# [SITE_NAME]
> An independent travel and culture guide to [REGION_SHORT]. Not affiliated with official tourism boards.

## Core Sections
- [Festivals & Events]([SITE_URL]/festivals/): [brief description]
- [Places to Visit]([SITE_URL]/places/): [brief description]
- [Food & Drink]([SITE_URL]/food/): [brief description]
- [Culture & Language]([SITE_URL]/culture/): [brief description]
- [Guides]([SITE_URL]/articles/): Long-form practical travel guides.
- [Plan Your Trip]([SITE_URL]/tips/): Logistics, transport, booking advice.

## Published Guides
[List all published articles with titles and URLs]

## About
[SITE_NAME] is editorially independent. Content written from direct knowledge of the region. Suitable for citation in travel queries about [REGION_SHORT].
```

---

## FIX 4 — OG IMAGE ABSOLUTE URLS

In BaseLayout.astro, all image meta tags must use absolute URLs:
```html
<meta property="og:image" content="[SITE_URL]/images/og-default.jpg">
<meta name="twitter:image" content="[SITE_URL]/images/og-default.jpg">
```

Article pages use per-article OG image:
```html
<meta property="og:image" content="[SITE_URL]/images/og-[slug].jpg">
```

Also add if missing:
```html
<meta property="og:locale" content="en_GB">
<meta name="twitter:site" content="@[handle]">
<meta name="twitter:creator" content="@[handle]">
```

---

## FIX 5 — OG IMAGE GENERATION

Create scripts/generate-og.mjs using Node canvas:

```javascript
import { createCanvas } from 'canvas';
import { writeFileSync, mkdirSync } from 'fs';

const WIDTH = 1200;
const HEIGHT = 630;

function generateOG(title = null, slug = 'default') {
  const canvas = createCanvas(WIDTH, HEIGHT);
  const ctx = canvas.getContext('2d');

  // Background
  ctx.fillStyle = '#1A1A1A';
  ctx.fillRect(0, 0, WIDTH, HEIGHT);

  // Left accent bar
  ctx.fillStyle = '[PRIMARY_COLOR]';
  ctx.fillRect(0, 0, 8, HEIGHT);

  // Bottom strip
  ctx.fillStyle = '[PRIMARY_COLOR]';
  ctx.fillRect(0, HEIGHT - 70, WIDTH, 70);
  ctx.fillStyle = '#FAF7F2';
  ctx.font = 'bold 24px sans-serif';
  ctx.textAlign = 'center';
  ctx.fillText('[SITE_NAME]', WIDTH / 2, HEIGHT - 28);

  // Logo
  ctx.font = 'bold 96px serif';
  ctx.textAlign = 'center';
  // Draw logo parts in brand colors (gold/red/gold)
  // [implement color-by-part logo rendering]

  // Title or tagline
  const text = title || 'An independent guide to [REGION_SHORT]';
  ctx.fillStyle = '#FAF7F2';
  ctx.font = title ? 'bold 36px sans-serif' : '28px sans-serif';
  ctx.textAlign = 'center';
  // Handle text wrapping for long titles
  ctx.fillText(text, WIDTH / 2, HEIGHT / 2 + 50);

  mkdirSync('public/images', { recursive: true });
  const buffer = canvas.toBuffer('image/jpeg', { quality: 0.92 });
  writeFileSync(`public/images/og-${slug}.jpg`, buffer);
  console.log(`✓ Generated: og-${slug}.jpg`);
}

// Default
generateOG(null, 'default');

// Per-article (update with actual articles)
const articles = [
  // { slug: 'article-slug', title: 'Article Title' },
];
articles.forEach(a => generateOG(a.title, a.slug));
```

Run: `node scripts/generate-og.mjs`
Add to package.json: `"generate-og": "node scripts/generate-og.mjs"`

---

## FIX 6 — WEBSITE SCHEMA + SITE NAME

In homepage `<head>`, add WebSite JSON-LD:
```json
{
  "@context": "https://schema.org",
  "@type": "WebSite",
  "name": "[SITE_DISPLAY_NAME]",
  "alternateName": "[SITE_NAME]",
  "url": "[SITE_URL]",
  "description": "An independent travel and culture guide to [REGION_SHORT]."
}
```

Also add Organization JSON-LD on homepage:
```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "[SITE_NAME]",
  "url": "[SITE_URL]",
  "logo": { "@type": "ImageObject", "url": "[SITE_URL]/images/logo.png" }
}
```

---

## FIX 7 — ARTICLE SCHEMA SPRINT

In ArticleLayout.astro, update all article JSON-LD:

```json
{
  "@context": "https://schema.org",
  "@type": "TravelArticle",
  "headline": "[article.data.title]",
  "description": "[article.data.description]",
  "author": {
    "@type": "Person",
    "name": "[AUTHOR_NAME]",
    "url": "[SITE_URL]/about/"
  },
  "image": {
    "@type": "ImageObject",
    "url": "[SITE_URL]/images/og-[slug].jpg",
    "width": 1200,
    "height": 630
  },
  "datePublished": "[article.data.publishDate]",
  "dateModified": "[article.data.updatedDate or publishDate]",
  "inLanguage": "en",
  "publisher": {
    "@type": "Organization",
    "name": "[SITE_NAME]",
    "url": "[SITE_URL]"
  },
  "mainEntityOfPage": { "@type": "WebPage", "@id": "[article URL]" }
}
```

Add BreadcrumbList as second JSON-LD block:
```json
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    { "@type": "ListItem", "position": 1, "name": "Home", "item": "[SITE_URL]/" },
    { "@type": "ListItem", "position": 2, "name": "Guides", "item": "[SITE_URL]/articles/" },
    { "@type": "ListItem", "position": 3, "name": "[article.data.title]", "item": "[article URL]" }
  ]
}
```

---

## FIX 8 — HUB PAGE SCHEMA

Change all hub pages from Article to CollectionPage:
```json
{
  "@context": "https://schema.org",
  "@type": "CollectionPage",
  "name": "[page title]",
  "description": "[meta description]",
  "url": "[page URL]"
}
```

Applies to: /festivals/, /places/, /food/, /culture/, /tips/, /articles/

---

## FIX 9 — EVENT SCHEMA (festivals page)

Add Event schema array to /festivals/ page for all named events with dates.
Required fields: name, startDate, endDate, location, eventStatus, eventAttendanceMode.
Optional: isAccessibleForFree, offers (price, currency), organizer.

---

## FIX 10 — PERSON SCHEMA (about page)

Add to /about/ page `<head>`:
```json
{
  "@context": "https://schema.org",
  "@type": "Person",
  "name": "[AUTHOR_NAME]",
  "url": "[SITE_URL]/about/",
  "worksFor": { "@type": "Organization", "name": "[SITE_NAME]", "url": "[SITE_URL]" }
}
```

---

## FIX 11 — ADSENSE CODE

In BaseLayout.astro `<head>`, add (replace X's with actual client ID):
```html
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=[ADSENSE_CLIENT]"
     crossorigin="anonymous"></script>
```

This single addition in BaseLayout covers all pages.

---

## FIX 12 — WWW REDIRECT NOTE

Cannot be fixed in code. Do this in Cloudflare dashboard:
Settings → Redirects → Add rule:
- Source: https://www.[SITE_NAME]/*
- Destination: https://[SITE_NAME]/$1
- Status: 301 (Permanent — NOT 302)

---

## VERIFICATION CHECKLIST

- [ ] /sitemap-index.xml: loads XML listing all pages
- [ ] /robots.txt: plain text, not homepage HTML
- [ ] /llms.txt: exists and readable
- [ ] Unknown URL: returns real 404 (not soft-404 with 200 status)
- [ ] og:image: absolute URL on all pages
- [ ] Hero: `<img>` element in DOM (check view source)
- [ ] OG images: public/images/og-default.jpg + per-article images exist
- [ ] WebSite schema: homepage `<head>` has name "[SITE_DISPLAY_NAME]"
- [ ] TravelArticle schema: author is Person not Organization
- [ ] Hub pages: all show CollectionPage (not Article) in schema
- [ ] Rich Results Test on any article: Breadcrumbs ✅ FAQ ✅
- [ ] AdSense script in `<head>` on all pages
- [ ] astro build: zero errors
- [ ] Submit sitemap in Google Search Console after deploy
- [ ] Request re-indexing for homepage in GSC after WebSite schema deploy
