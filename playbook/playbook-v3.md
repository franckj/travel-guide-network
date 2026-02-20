# Regional Travel Guide — Operations Playbook
**v3.0 · February 2026**
*Based on ilovecatalonia.com · Template for VisitProsecco.com and beyond*

---

## 1. The Concept

This playbook documents how to build, launch, and grow an independent regional travel and culture guide — using ilovecatalonia.com as the reference build. The model: own a high-value geographic domain, build a content-rich editorial site that ranks for traveler search intent, and monetize through multiple channels without depending on any single one.

The key insight is that tourism boards produce brochures, travel aggregators produce listings, and bloggers produce opinions. None of them produce what travelers actually need: opinionated, data-backed, insider guides that treat the reader as intelligent. That gap is where these sites live.

### What Makes This Model Work

**Domain Authority** — A strong domain (ilovecatalonia.com, VisitProsecco.com) signals intent before Google even reads the content. Exact-match and near-match domains still carry weight for regional travel queries. The domain is the brand.

**Editorial Voice** — Every article is written with a point of view. Not "here are 10 things to do" but "here's what locals know that tourists don't." This tone builds trust, reduces bounce rate, and creates return visitors — all signals Google rewards.

**Content Depth** — Each guide targets 2,500–4,000 words with real data, comparison tables, stat strips, and FAQ sections. This length signals authority and captures featured snippets, PAA boxes, and long-tail variations naturally.

**Revenue Diversity** — No single revenue stream. Traffic monetizes through display ads, affiliate links (tours, accommodation, transport), sponsored content from local tourism boards, and eventually digital products. Losing one stream does not kill the site.

### The Sites This Works For

The template applies to any region with:
1. Meaningful English-language tourist search volume (50K+ monthly searches)
2. Underserved existing content — generic listicles, outdated tourism board pages
3. A distinct identity worth writing about with depth

Target metrics: competitor DA under 40, 3–6 months to first traffic, 12–18 months to monetization.

---

## 2. Tech Stack

| Layer | Tool | Why |
|-------|------|-----|
| Framework | Astro.build | Zero JS by default — fastest load times. Static output. Built-in content collections. |
| Styling | Tailwind CSS v4 | Utility-first, no unused CSS in production, easy to maintain without a designer. |
| Content | Astro Content Collections | Markdown files with typed schemas. Non-developers can add content without touching code. |
| Hosting | Cloudflare Pages | Global CDN, free tier generous, Workers for edge logic, instant deploys from GitHub. |
| SEO | astro-seo + rehype-slug | Meta tags, OG tags, canonical URLs, JSON-LD, TOC anchor IDs — all automated. |
| Analytics | Plausible | Privacy-first, no cookie banner needed for EU visitors, lightweight. |
| Images | Unsplash API / Pexels | Free high-quality travel photography. |
| i18n | Astro i18n routing | Route structure (/en/, /ca/, /es/) set up from day one even if only English is live. |

### Component Library

These 5 components handle 90% of visual work across all pages:

| Component | Purpose | Used In |
|-----------|---------|---------|
| StatBar | 4-stat horizontal strip (gold bg, off-white text) | Every guide article, hub pages |
| ComparisonTable | Styled table with semantic cell colors (€ = gold, Free = green, UNESCO = red) | Festival guides, place comparisons |
| FAQAccordion | Native details/summary, FAQPage JSON-LD injected automatically | Bottom of every guide |
| TipBox | Highlighted callout for insider tips | Within article body |
| CardGrid | 3-column grid of content cards with hover state | Hub pages, related articles |

---

## 3. SEO Strategy

### Keyword Selection Framework

| Criterion | Target | Why |
|-----------|--------|-----|
| Monthly search volume | 500–5,000 per keyword | Big enough to drive traffic, small enough to rank without DA |
| Competitor DA | Under 40 | Independent sites can compete against tourism boards and listicle farms |
| Search intent | Informational / navigational | Travel research queries, not transactional |
| Content gap | Thin or outdated results | Pages ranking with 500-word listicles from 2019 are beatable |

### Content Priority Order

| Phase | Content Type | Target | Timeline |
|-------|-------------|--------|----------|
| 1 | Hub pages (6) | Topical authority across all main sections | Week 1–2 |
| 2 | Mega article | Primary money keyword — 3,000+ words | Week 2–3 |
| 3 | Supporting guides (3) | Long-tail variations of money keyword | Week 3–5 |
| 4 | Comparison articles | "X vs Y" and "Best X in Region" formats | Week 5–7 |
| 5 | Itinerary content | "X days in Region" — high-intent trip planning | Month 2–3 |

### On-Page SEO Checklist

- Title tag: `[Page Title] | [SITE_NAME]` — max 60 chars
- Meta description: 120–155 chars, includes primary keyword naturally
- H1: one per page, matches search intent
- H2s: each targets a related query or subtopic
- Internal links: every article links to 2+ hub pages + 1–2 related articles
- External links: 2–3 authority sources per hub page (UNESCO, official sites, government data)
- Schema: correct type per page (TravelArticle, CollectionPage, Event, Person)
- FAQPage schema: on every article and hub page
- BreadcrumbList schema: on every article page
- Images: descriptive alt text, OG image per page (absolute URL)
- "Last updated" date visible on every article

---

## 4. Content Guidelines

Content quality is the only durable competitive advantage. Tourism boards have budget. Aggregators have scale. Independent sites have voice and depth. Never compete on quantity — compete on quality.

### Editorial Voice

| Do | Don't |
|----|-------|
| "La Boqueria is worth seeing once but you'll eat better at Santa Caterina" | "La Boqueria is a must-visit Barcelona landmark" |
| "Arrive at Castellers at least an hour early — the crowd builds fast" | "Castellers are a unique Catalan tradition you shouldn't miss" |
| "The Catalan language has 10 million speakers — calling it a dialect is offensive" | "Catalan people speak both Catalan and Spanish" |
| "Skip the waterfront paella — it's almost certainly frozen" | "Barcelona has many excellent seafood restaurants" |

### Article Structure Template

1. Intro (2–3 paragraphs): hook, thesis, why this matters
2. StatBar component: 4 key stats relevant to the topic
3. Direct-answer lede: 2 sentences answering the implicit query
4. Main content sections: H2 headings, 300–600 words each
5. ComparisonTable: at least one per article
6. Budget section: actual price ranges, not vague estimates
7. TipBox callouts: 3–5 per article
8. FAQ section: FAQAccordion, 7+ questions from real traveler forums
9. Related articles: auto-generated by ArticleLayout (exclude current)

### Word Count Targets

| Content Type | Target Words | Notes |
|-------------|-------------|-------|
| Hub pages | 800–1,200 | Direct-answer ledes matter more than volume |
| Mega article | 3,000–4,000 | Primary money keyword — depth is the advantage |
| Supporting guides | 2,500–3,500 | Long-tail intent — be specific, not comprehensive |
| Comparison articles | 1,500–2,500 | Clear verdict + data — make the decision for the reader |
| Itinerary articles | 2,000–3,000 | Day-by-day structure — practical and specific |

---

## 5. Monetization

### Revenue Stream Timeline

| Stream | Activates At | Monthly Potential | Implementation |
|--------|-------------|-------------------|----------------|
| Display ads (AdSense) | Any — apply early | 50–500 EUR | Apply after 5+ articles + required pages |
| Display ads (Mediavine) | 50K sessions/mo | 500–2,000 EUR+ | One-line script, automatic |
| Affiliate — accommodation | 5K sessions/mo | 200–800 EUR | Booking.com, GetYourGuide |
| Affiliate — tours | 5K sessions/mo | 150–600 EUR | GetYourGuide, Viator, Airbnb Exp. |
| Affiliate — transport | Any | 50–200 EUR | Omio, Rentalcars, FlixBus |
| Sponsored guides | DA 20+, 10K+ traffic | 500–3,000 EUR/piece | Tourism boards, wine consortiums |
| Digital products | Established audience | 300–2,000 EUR/mo | Itinerary PDFs, local guides |
| Newsletter sponsorship | 2,000+ subscribers | 200–1,000 EUR/issue | Local businesses, tour operators |

### Monetization Rules

- **Never recommend something you would not recommend without payment.**
- AdSense and affiliate links disclosed in footer — not per link, not aggressively.
- Sponsored content labeled "In partnership with [X]" — no exceptions.
- Display ads: maximum 3 per page, never mid-article.
- No pop-ups. Ever.

---

## 6. Launching a New Site

The ilovecatalonia.com codebase is the template. Every new site reuses the same Astro structure, component library, and content collection schemas. Only the domain, color palette, content, and monetization focus change.

### Variables to Update for Each New Site

Update the `VARIABLES` block at the top of each prompt file before running:

```
SITE_NAME         = "VisitProsecco.com"
SITE_URL          = "https://visitprosecco.com"
SITE_DISPLAY_NAME = "Visit Prosecco"
REGION            = "Prosecco Hills, Veneto, Italy"
REGION_SHORT      = "Prosecco"
AUTHOR_NAME       = "Franck"
CONTACT_EMAIL     = "hello@visitprosecco.com"
PRIMARY_COLOR     = "#2C5F2E"  # vineyard green
ACCENT_COLOR      = "#D4A017"  # gold — keep across all sites
FONT_DISPLAY      = "Cormorant Garamond"
FONT_BODY         = "Lato"
MONEY_KEYWORD     = "visiting Prosecco Italy guide"
AFFILIATE_FOCUS   = "winery tours, private drivers, agriturismo"
TOURISM_BOARD     = "Veneto Tourism Board"
```

### Site Network — Candidate Domains

| Domain | Region | Primary Hook | Top Affiliate |
|--------|--------|-------------|---------------|
| VisitProsecco.com | Veneto, Italy | UNESCO wine hills, 90 min from Venice | Winery tours 80–200 EUR/pp |
| ILoveSicily.com | Sicily, Italy | Food, archaeology, beaches, film | Cooking classes, transfers |
| ILoveBasqueCountry.com | Basque, Spain | Pintxos, Guggenheim, surf, identity | Food tours, surf schools |
| ILoveMaltese.com | Malta | History, diving, English-speaking | Dive tours, boat trips |
| ILoveSlovenia.com | Slovenia | Lake Bled, caves, wine, small and safe | Multi-day tours, hiking |
| ILoveFaroe.com | Faroe Islands | Epic scenery, zero mass tourism | Guided hikes, boat tours |

---

## 6A. Pre-Launch Checklist (v2 — Post-Audit)

*Incorporates all lessons from the ilovecatalonia.com SEO audit (Feb 2026, starting score: 53/100).*

### Week 1 — Foundation + Infrastructure

- [ ] Domain acquired and pointed to Cloudflare
- [ ] GitHub repo created, Cloudflare Pages connected
- [ ] Astro project from PROMPT-1, brand tokens updated
- [ ] Google Search Console property created and verified
- [ ] Plausible Analytics installed
- [ ] @astrojs/sitemap installed, site URL set in astro.config.mjs
- [ ] robots.txt deployed — plain text, not homepage HTML (test: `curl -I [URL]/robots.txt`)
- [ ] llms.txt deployed at /llms.txt
- [ ] 404 page created — unknown paths return real 404 status (not soft-404)
- [ ] www → non-www 301 redirect in Cloudflare dashboard (not 302)
- [ ] og:image and twitter:image set to absolute URLs in BaseLayout.astro
- [ ] og:locale, twitter:site, twitter:creator added
- [ ] Hero converted from CSS background to `<img>` with `fetchpriority="high"`
- [ ] `<link rel="preload">` for hero image in `<head>`
- [ ] OG images generated: 1 default (1200×630px) + 1 per article
- [ ] Organization + WebSite JSON-LD on homepage
- [ ] Homepage Latest Guides section links to 3 most recent articles

### Week 1 Writing — Required Before Any Ad Application

- [ ] About page at /about/ — real author name + editorial mission (400–600 words)
- [ ] Contact page at /contact/ — working email address
- [ ] Privacy Policy at /privacy/ — AdSense cookie disclosure + GDPR contact
- [ ] Terms of Service at /terms/
- [ ] Disclaimer at /disclaimer/ — affiliate disclosure
- [ ] All 5 pages linked from footer Legal column

### Week 2–3 — Content + Schema

- [ ] 5–6 hub pages with correct SEO titles and meta descriptions
- [ ] All hub pages: CollectionPage schema (NOT Article type)
- [ ] 1 mega article (3,000+ words, primary money keyword)
- [ ] All articles: TravelArticle schema with Person author + image + BreadcrumbList
- [ ] Event schema on festivals page for all named events
- [ ] Rich Results Test passes on 1+ article and 1+ hub page (Breadcrumbs + FAQ)
- [ ] FAQPage schema on all hub pages and articles
- [ ] 2–3 external authority citations per hub page
- [ ] "Last updated" date visible on all articles
- [ ] Author byline on all articles linking to /about/
- [ ] Direct-answer lede on every hub page
- [ ] Internal linking: every article links to 2+ hub pages

### Week 4–6 — Content Expansion + Monetization

- [ ] 3 supporting guide articles published
- [ ] 1 comparison article published
- [ ] Affiliate links integrated (Booking.com, GetYourGuide)
- [ ] Email signup form live — offer free PDF itinerary
- [ ] Social profiles created: Instagram, Pinterest
- [ ] Apply for AdSense (all Week 1 writing tasks must be complete first)
- [ ] Set up Google CMP consent banner for EEA visitors after AdSense approval

---

## 7. Quick Reference

### The 5 Prompt Files

| # | File | When to Run | What It Builds |
|---|------|-------------|----------------|
| 1 | PROMPT-1-base-build.md | Blank repo — first | Full Astro site: layouts, components, all hub pages, homepage, infrastructure |
| 2 | PROMPT-2-articles.md | After base build deployed | 5 long-form articles, FAQ sections, schema verification, internal linking |
| 3 | PROMPT-3-ui-fixes.md | After visual review | Logo, hero, nav states, StatBar, TOC, related articles, card truncation, breadcrumbs |
| 4 | PROMPT-4-seo-infrastructure.md | After UI fixes, before AdSense | Schema sprint, OG images, WebSite schema, AdSense script |
| 5 | PROMPT-5-required-pages.md | Before AdSense application | About, Contact, Privacy, Terms, Disclaimer. Footer Legal column. Author bylines. |

### SEO Title Formulas

- Every page: `[Page Title] | [SITE_NAME]`
- Homepage: `[Region] Travel & Culture Guide | [SITE_NAME]`
- Festivals hub: `Festivals in [Region] [Year]: The Complete Guide | [SITE_NAME]`
- Places hub: `Best Places to Visit in [Region] Beyond [Main City] | [SITE_NAME]`
- Food hub: `[Region] Food & Drink Guide: What to Eat | [SITE_NAME]`
- Culture hub: `[Region] Culture, Language & Identity | [SITE_NAME]`
- Tips hub: `Practical Tips for Visiting [Region] | [SITE_NAME]`
- Guides hub: `In-Depth Guides to [Region] | [SITE_NAME]`

### Monthly Maintenance

1. Check Google Search Console for impressions, clicks, new ranking pages
2. Update articles mentioning current-year events (festival dates, prices, lineups)
3. Add 1 new long-form guide per month minimum
4. Check affiliate link validity — broken links kill conversion
5. Review top 5 pages for internal link opportunities to newer content
6. Check Core Web Vitals — Astro + Cloudflare should keep this green

### Tools & Costs

| Tool | Purpose | Cost |
|------|---------|------|
| Cloudflare Pages | Hosting + CDN | Free tier |
| Plausible Analytics | Privacy-first analytics | 9 EUR/mo |
| Kit (ConvertKit) | Email list | Free to 1,000 subs |
| Ahrefs Webmaster Tools | SEO monitoring | Free for own sites |
| Google Search Console | Search performance | Free |
| Google AdSense | Display ads (early stage) | Free — revenue share |
| Mediavine | Display ads (50K+ sessions) | Free — revenue share |
| GetYourGuide Partner | Tour affiliates | Free, 8% commission |
| Booking.com Affiliate | Accommodation | Free, 4–6% commission |

---

## 8. SEO & Technical Readiness

### 8.1 Required Pages — Day One

| Page | URL | Why Required |
|------|-----|-------------|
| About | /about/ | E-E-A-T: named author, editorial mission, human credibility signal |
| Contact | /contact/ | Trust signal for Google quality raters and AdSense reviewers |
| Privacy Policy | /privacy/ | GDPR compliance + required AdSense cookie disclosure |
| Terms of Service | /terms/ | Legal protection, expected by all ad networks |
| Disclaimer | /disclaimer/ | Affiliate disclosure required by FTC and ad networks |

### 8.2 Schema Standards

| Page Type | Correct Schema | Common Mistake |
|-----------|---------------|----------------|
| Article / Guide | TravelArticle | Using generic "Article" |
| Hub page | CollectionPage | Using "Article" — wrong type |
| Homepage | WebSite + Organization | No schema at all |
| About page | Person | No schema at all |
| Festival listing | Event (array) | Missing — highest ROI schema on travel sites |

### 8.3 E-E-A-T Minimum Bar

| Dimension | Requirement |
|-----------|-------------|
| Experience | First-person voice. "Last updated" date on all articles. |
| Expertise | Author bio with real name on /about/. Byline on every article linking to /about/. |
| Authoritativeness | 2–3 external citations per hub page (UNESCO, official sites, government data). |
| Trustworthiness | Working contact email. Privacy Policy with AdSense disclosure. Disclaimer with affiliate disclosure. |

### 8.4 GEO / AI Search Readiness

For visibility in AI-powered search (ChatGPT, Perplexity, Claude):

- llms.txt at root listing all sections and published guides
- Direct-answer ledes: 2 sentences answering the implicit query before editorial content
- Name the site as source: "According to ilovecatalonia.com..." style phrasing in body
- FAQPage schema on all hub pages

### 8.5 30-Day Score Projection

| Milestone | Score | Timeline |
|-----------|-------|----------|
| Launch (PROMPT-1 infrastructure) | ~62/100 | Day 0 |
| After articles + schema (PROMPT-2 + PROMPT-4) | ~70/100 | Day 7–10 |
| After required pages (PROMPT-5) | ~74/100 | Day 10–14 |
| After content + GEO fixes | ~78/100 | Day 20–30 |
| AdSense application ready | — | Day 5–7 |

> ilovecatalonia.com went from 53/100 with broken infrastructure to AdSense-verified in a single evening. The next site starts at ~62 because PROMPT-1 bakes in everything that was fixed post-audit.
