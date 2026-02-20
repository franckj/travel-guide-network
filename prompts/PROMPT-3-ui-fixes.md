# PROMPT-3-ui-fixes.md
# Regional Travel Guide Network — UI Fixes (All Rounds)
# Run after visual review of the base build. Apply targeted fixes only.
# Consolidates all lessons from ilovecatalonia.com UI review rounds 1–4.

---

## VARIABLES — UPDATE BEFORE RUNNING

```
PRIMARY_COLOR    = "#C1001F"   # e.g. Catalan red
ACCENT_COLOR     = "#D4A017"   # Gold
BG_COLOR         = "#FAF7F2"   # Off-white
TEXT_COLOR       = "#1A1A1A"   # Charcoal
LOGO_TEXT        = "i♥catalonia"
```

---

## IMPORTANT: DO NOT REBUILD

Apply targeted fixes only. Do not restructure layouts, rename files,
or change content. Run `astro build` after all fixes and report errors.

---

## FIX 1 — LOGO: Brand Color Treatment

Logo must read: [LOGO_TEXT]
- "i" = [ACCENT_COLOR] gold
- "♥" = [PRIMARY_COLOR] red (HTML entity or inline SVG)
- site name = [ACCENT_COLOR] gold

Apply in both Nav.astro and Footer.astro logo instances.
Heart should sit on the text baseline, sized to match surrounding text.

---

## FIX 2 — HERO: Correct Height and Element Type

Hero must be an `<img>` element (NOT CSS background-image).
CSS backgrounds are not crawlable by Google.

```html
<img
  src="/images/[region]-hero.jpg"
  alt="[Descriptive alt text for the region]"
  width="1440"
  height="800"
  fetchpriority="high"
  class="[layout classes]"
/>
```

Add to `<head>` in BaseLayout.astro:
```html
<link rel="preload" as="image" href="/images/[region]-hero.jpg">
```

Height constraints:
- Remove any min-height: 100vh or height: 100vh
- Desktop: min-height: 480px
- Mobile: min-height: 360px
- Hero heading: max 3.5rem desktop, 2.2rem mobile
- CTA buttons must be visible without scrolling on 768px-tall screen

---

## FIX 3 — NAVIGATION: Both States

Default state (unscrolled, over light background):
```css
nav a { color: [TEXT_COLOR]; }
nav a.active { color: [PRIMARY_COLOR]; }
```

Scrolled state (past 60px, dark background):
```css
nav.scrolled { background: #1A1A1A; }
nav.scrolled a { color: #FAF7F2; }
nav.scrolled a.active { color: [ACCENT_COLOR]; }
```

Scroll listener:
```javascript
const nav = document.querySelector('nav');
window.addEventListener('scroll', () => {
  nav.classList.toggle('scrolled', window.scrollY > 60);
});
```

Also check:
- Mobile hamburger touch target: minimum 48×48px
- No content hidden behind sticky nav — add scroll-margin-top: 88px to all H2/H3

---

## FIX 4 — STATBAR: Colors

Background: [ACCENT_COLOR]
Text (values): #FAF7F2 (off-white) — NOT dark on gold
Text (labels): #FAF7F2 at 85% opacity
Mobile: 2×2 grid, reduced font sizes (values 1.4rem, labels 0.7rem)
Desktop: 4-column row, values 2rem, labels 0.8rem

---

## FIX 5 — NAV LABEL "GUIDES"

The /articles/ URL displays as "Guides" in navigation everywhere.
- Nav link text: "Guides"
- Page title: "Guides"
- H1: "Guides"
- Breadcrumbs: "Guides"
- URL /articles/ stays unchanged — do NOT rename routes

---

## FIX 6 — TOC SIDEBAR: Correct Scope

TOC must query H2 elements inside `<article>` only — not the whole document:

```javascript
// WRONG
document.querySelectorAll('h2')

// CORRECT
document.querySelector('article').querySelectorAll('h2')
```

Full TOC script with active section highlighting:
```javascript
const article = document.querySelector('article');
const headings = article ? article.querySelectorAll('h2') : [];
const tocList = document.getElementById('toc-list');

if (tocList && headings.length > 0) {
  headings.forEach(heading => {
    if (!heading.id) {
      heading.id = heading.textContent
        .toLowerCase()
        .replace(/[^a-z0-9]+/g, '-')
        .replace(/(^-|-$)/g, '');
    }
    const li = document.createElement('li');
    const a = document.createElement('a');
    a.href = `#${heading.id}`;
    a.textContent = heading.textContent;
    li.appendChild(a);
    tocList.appendChild(li);
  });
}

const observer = new IntersectionObserver(entries => {
  entries.forEach(entry => {
    const id = entry.target.getAttribute('id');
    const link = tocList?.querySelector(`a[href="#${id}"]`);
    if (link) link.classList.toggle('active', entry.isIntersecting);
  });
}, { rootMargin: '-20% 0% -70% 0%' });

headings.forEach(h => observer.observe(h));
```

Add `id="toc-list"` to the `<ul>` in the TOC HTML.

---

## FIX 7 — RELATED ARTICLES: Exclude Current Article

```astro
---
const { slug, category } = Astro.props;
const allArticles = await getCollection('articles');

const related = allArticles
  .filter(a => a.slug !== slug)           // exclude self
  .filter(a => a.data.category === category)
  .slice(0, 3);

const fallback = allArticles
  .filter(a => a.slug !== slug)
  .filter(a => a.data.category !== category)
  .slice(0, 3 - related.length);

const relatedArticles = [...related, ...fallback].slice(0, 3);
---
```

Ensure `slug` is passed as prop from `[slug].astro`:
```astro
<ArticleLayout slug={article.slug} category={article.data.category} ...>
```

---

## FIX 8 — CARD EXCERPT: Word Boundary Truncation

```typescript
function truncate(text: string, max = 120): string {
  if (text.length <= max) return text;
  return text.slice(0, text.lastIndexOf(' ', max)) + '…';
}
```

Apply wherever article description/excerpt is rendered in cards.

---

## FIX 9 — BREADCRUMBS

- Remove text-overflow: ellipsis / white-space: nowrap on last breadcrumb item
- Full title should display on desktop without truncation
- Reduce gap between breadcrumb and article title to 1.5rem max

---

## FIX 10 — ARTICLE HEADER WHITESPACE

Reduce margin between article meta (date, reading time, author) and
first paragraph of body text to 1.5rem maximum.

---

## VERIFICATION CHECKLIST

- [ ] Logo: correct color treatment in nav and footer
- [ ] Hero: `<img>` element (not CSS background), visible above fold on 768px screen
- [ ] Nav default state: dark text, visible on light background
- [ ] Nav scrolled state: white text on #1A1A1A background
- [ ] Active link: [PRIMARY_COLOR] unscrolled, [ACCENT_COLOR] scrolled
- [ ] StatBar: [ACCENT_COLOR] background, off-white text
- [ ] Nav label "Guides" (not "Articles") — URL /articles/ unchanged
- [ ] TOC shows current article H2s only (not other page titles)
- [ ] TOC active section highlights on scroll
- [ ] Related articles: 3 cards, none is the current article
- [ ] Card excerpts: no mid-word truncation, clean ~120 char cut
- [ ] Breadcrumbs: not truncated on desktop
- [ ] No content hidden behind sticky nav
- [ ] astro build: zero errors
