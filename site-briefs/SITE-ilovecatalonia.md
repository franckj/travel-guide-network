# Site Brief: ilovecatalonia.com

## Status
Active — in development. Deployed at ilovecatalonia.pages.dev.
Production domain: ilovecatalonia.com (confirm DNS status)

## Identity
- **Tagline:** An independent travel & culture guide to one of Europe's most extraordinary regions
- **Voice:** Opinionated insider. Not affiliated with tourism boards. Treats readers as intelligent adults.
- **Disclaimer in footer:** "Not affiliated with the Catalan Tourism Board — just obsessed with the place."

## Brand Tokens
- Primary color: `#C1001F` (Catalan red)
- Accent / StatBar: `#D4A017` (gold)
- Background: `#FAF7F2` (warm off-white)
- Dark: `#1A1A1A`
- Display font: Playfair Display (or Cormorant Garant)
- Body font: Source Sans 3

## Logo
`i♥catalonia` — "i" gold, "♥" red, "catalonia" gold. On dark nav background.

## Tech Stack
- Framework: Astro.build
- Styling: Tailwind CSS v4
- Hosting: Cloudflare Pages (auto-deploy from GitHub)
- Analytics: Fathom or Plausible (privacy-first)
- Content: Astro Content Collections (.md files)

## Hub Sections (URL structure)
- `/festivals/` — Festivals & Events
- `/places/` — Places to Visit
- `/food/` — Food & Drink
- `/culture/` — Culture & Language
- `/tips/` — Plan Your Trip
- `/articles/` — displayed as "Guides" in nav

## Articles Published (as of Feb 2026)
1. `catalan-food-vs-spanish-food` — Food & Drink
2. `la-merce-festival-guide` — Festivals & Events
3. `girona-guide` — Places to Visit
4. `catalan-language-guide` — Culture & Language
5. `one-week-catalonia-itinerary` — Practical Tips

## Articles Pending (from addendum prompt — not yet built)
6. Music Festivals in Catalonia 2026 (target: "music festivals Catalonia 2026")
7. Traditional Festivals in Catalonia (target: "traditional festivals Catalonia")
8. Primavera Sound Barcelona 2026 (target: "Primavera Sound 2026 guide")
9. Sónar Festival Barcelona 2026 (target: "Sónar festival guide")
10. Catalonia Beyond Barcelona (target: "Catalonia beyond Barcelona")

## UI Status (all fixes applied as of Feb 2026)
- ✅ Logo: gold + red heart
- ✅ Nav: dark background when scrolled, dark text when unscrolled
- ✅ Footer: multilingual line + Made with ♥ for Catalunya + Legal column
- ✅ StatBar: gold bg, off-white text
- ✅ Related articles: real cards (3 per article, excludes current)
- ✅ Breadcrumbs: no truncation
- ✅ TOC: scoped to article element only, active section highlights
- ✅ FAQ sections: live on all articles
- ✅ Site name: "I ♥ Catalonia" via WebSite schema
- ✅ Title suffix: "| ilovecatalonia.com" on all pages

## SEO Status (as of Feb 2026)
- Score: 53/100 at launch → ~76/100 target (30 days)
- ✅ sitemap.xml live, submitted to GSC
- ✅ robots.txt plain text with AI crawler rules
- ✅ llms.txt live
- ✅ OG images: default + per-article (1200×630px)
- ✅ TravelArticle schema with Person author on all articles
- ✅ CollectionPage schema on all hub pages
- ✅ Event schema on /festivals/
- ✅ Rich Results Test: Breadcrumbs + FAQ valid
- ✅ 3 pages indexed in Google at launch (homepage, La Mercè, food article)
- ⬜ Week 3: direct-answer ledes on hubs, external citations, FAQPage on remaining hubs
- ⬜ Week 4: hreflang architecture (/ca/, /es/)

## Monetization Status
- ✅ AdSense applied and site verified (Feb 2026)
- ✅ Google CMP consent banner set up for EEA visitors
- ⬜ Awaiting AdSense approval (24–72 hours typical)
- ⬜ Mediavine: target once 50K sessions/month reached
- ⬜ Affiliate links (GetYourGuide, Booking.com): add when articles reach page 2+

## Known Issues / Decisions
- `/articles/` URL stays as-is; displayed as "Guides" everywhere in nav/UI
- No StatBar on articles — removed intentionally (too repetitive)
- Internal linking is deliberate — follow silo structure, don't cross-link randomly
- Game of Thrones references intentionally omitted — feels dated, not the site's angle

---
