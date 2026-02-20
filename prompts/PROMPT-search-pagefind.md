# Claude Code Prompt: ilovecatalonia.com — Pagefind Search

Targeted addition only. Do not rebuild any existing components or layouts.
Run `astro build` after all changes and report any errors.

---

## OVERVIEW

Add a full-text search feature using Pagefind — a static search library that
indexes content at build time and runs entirely in the browser.
No external service, no API keys, zero ongoing cost.

The search UI will appear as a modal overlay triggered by:
- A search icon in the nav bar
- The keyboard shortcut `/` (standard editorial site convention)
- The keyboard shortcut `Cmd+K` / `Ctrl+K`

---

## STEP 1 — Install Pagefind

```bash
npm install @pagefind/default-ui
```

Add a post-build script to package.json:
```json
{
  "scripts": {
    "build": "astro build && npx pagefind --site dist",
    "dev": "astro dev",
    "preview": "astro preview"
  }
}
```

This runs Pagefind indexing automatically after every `astro build`.

---

## STEP 2 — Mark Content for Indexing

In `ArticleLayout.astro`, add `data-pagefind-body` to the main article element:
```html
<article data-pagefind-body>
  <!-- all article content -->
</article>
```

In hub pages (festivals/index.astro, places/index.astro, etc.), add
`data-pagefind-body` to the main content section.

Exclude nav and footer from indexing — add `data-pagefind-ignore` to:
```html
<nav data-pagefind-ignore>...</nav>
<footer data-pagefind-ignore>...</footer>
```

Add metadata for richer search results. In ArticleLayout.astro `<head>`:
```html
<meta data-pagefind-meta="type" content="article">
```

On hub pages:
```html
<meta data-pagefind-meta="type" content="guide">
```

---

## STEP 3 — Build the Search Modal Component

Create `src/components/SearchModal.astro`:

```astro
---
// No props needed
---

<!-- Search Trigger Button (goes in Nav) -->
<!-- This component also exports the modal itself -->

<div id="search-modal-overlay" class="search-overlay" aria-hidden="true">
  <div class="search-modal" role="dialog" aria-label="Search">
    <div class="search-modal-header">
      <span class="search-modal-label">Search</span>
      <button id="search-close" class="search-close" aria-label="Close search">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <line x1="18" y1="6" x2="6" y2="18"></line>
          <line x1="6" y1="6" x2="18" y2="18"></line>
        </svg>
      </button>
    </div>
    <div id="search-container"></div>
    <div class="search-modal-footer">
      <span>Press <kbd>Esc</kbd> to close · <kbd>/</kbd> to open</span>
    </div>
  </div>
</div>

<link href="/pagefind/pagefind-ui.css" rel="stylesheet">

<style>
  /* Overlay */
  .search-overlay {
    position: fixed;
    inset: 0;
    background: rgba(26, 26, 26, 0.85);
    backdrop-filter: blur(4px);
    z-index: 1000;
    display: flex;
    align-items: flex-start;
    justify-content: center;
    padding-top: 10vh;
    opacity: 0;
    visibility: hidden;
    transition: opacity 0.2s ease, visibility 0.2s ease;
  }

  .search-overlay.active {
    opacity: 1;
    visibility: visible;
  }

  /* Modal box */
  .search-modal {
    background: #FAF7F2;
    border-radius: 8px;
    width: 100%;
    max-width: 640px;
    margin: 0 1.5rem;
    box-shadow: 0 24px 64px rgba(0, 0, 0, 0.4);
    overflow: hidden;
  }

  .search-modal-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 1rem 1.25rem 0.75rem;
    border-bottom: 1px solid #E8E2DA;
  }

  .search-modal-label {
    font-size: 0.75rem;
    font-weight: 700;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: #888;
  }

  .search-close {
    background: none;
    border: none;
    cursor: pointer;
    color: #888;
    padding: 4px;
    display: flex;
    align-items: center;
    border-radius: 4px;
    transition: color 0.15s, background 0.15s;
  }

  .search-close:hover {
    color: #1A1A1A;
    background: #E8E2DA;
  }

  .search-modal-footer {
    padding: 0.75rem 1.25rem;
    border-top: 1px solid #E8E2DA;
    font-size: 0.75rem;
    color: #888;
  }

  .search-modal-footer kbd {
    background: #E8E2DA;
    border-radius: 3px;
    padding: 1px 5px;
    font-family: monospace;
    font-size: 0.7rem;
    color: #1A1A1A;
  }

  /* Pagefind UI overrides — match site palette */
  #search-container {
    padding: 0.75rem 1.25rem 1rem;
  }

  #search-container :global(.pagefind-ui__search-input) {
    background: #fff;
    border: 2px solid #E8E2DA;
    border-radius: 6px;
    color: #1A1A1A;
    font-family: inherit;
    font-size: 1rem;
    padding: 0.75rem 1rem;
    width: 100%;
    outline: none;
    transition: border-color 0.15s;
  }

  #search-container :global(.pagefind-ui__search-input:focus) {
    border-color: #C1001F;
  }

  #search-container :global(.pagefind-ui__search-clear) {
    color: #888;
  }

  #search-container :global(.pagefind-ui__result) {
    border-bottom: 1px solid #E8E2DA;
    padding: 1rem 0;
  }

  #search-container :global(.pagefind-ui__result-link) {
    color: #1A1A1A;
    font-weight: 600;
    text-decoration: none;
    font-size: 1rem;
  }

  #search-container :global(.pagefind-ui__result-link:hover) {
    color: #C1001F;
  }

  #search-container :global(.pagefind-ui__result-excerpt) {
    color: #555;
    font-size: 0.875rem;
    margin-top: 0.25rem;
    line-height: 1.5;
  }

  #search-container :global(mark) {
    background: #FFF3D4;
    color: #1A1A1A;
    border-radius: 2px;
    padding: 0 2px;
  }

  #search-container :global(.pagefind-ui__button) {
    background: #C1001F;
    color: #FAF7F2;
    border: none;
    border-radius: 4px;
    padding: 0.5rem 1rem;
    cursor: pointer;
    font-family: inherit;
    font-size: 0.875rem;
  }

  #search-container :global(.pagefind-ui__button:hover) {
    background: #A0001A;
  }
</style>

<script>
  import('/pagefind/pagefind-ui.js').then(({ PagefindUI }) => {
    new PagefindUI({
      element: '#search-container',
      showSubResults: false,
      resetStyles: true,
      autofocus: true,
    });
  });

  const overlay = document.getElementById('search-modal-overlay');
  const closeBtn = document.getElementById('search-close');

  function openSearch() {
    overlay.classList.add('active');
    overlay.setAttribute('aria-hidden', 'false');
    document.body.style.overflow = 'hidden';
    // Focus the input after the modal animates in
    setTimeout(() => {
      const input = overlay.querySelector('.pagefind-ui__search-input');
      if (input) input.focus();
    }, 100);
  }

  function closeSearch() {
    overlay.classList.remove('active');
    overlay.setAttribute('aria-hidden', 'true');
    document.body.style.overflow = '';
  }

  // Close button
  closeBtn.addEventListener('click', closeSearch);

  // Click outside modal to close
  overlay.addEventListener('click', (e) => {
    if (e.target === overlay) closeSearch();
  });

  // Keyboard shortcuts
  document.addEventListener('keydown', (e) => {
    // Open with / or Cmd+K / Ctrl+K
    if ((e.key === '/' && !e.metaKey && !e.ctrlKey && !['INPUT', 'TEXTAREA'].includes(document.activeElement.tagName)) ||
        ((e.metaKey || e.ctrlKey) && e.key === 'k')) {
      e.preventDefault();
      openSearch();
    }
    // Close with Escape
    if (e.key === 'Escape') closeSearch();
  });

  // Expose openSearch globally so Nav button can call it
  window.openSearch = openSearch;
</script>
```

---

## STEP 4 — Add Search Icon to Nav

In `Nav.astro`, add a search button to the right side of the nav bar
(before or after the existing nav links):

```astro
<button
  class="nav-search-btn"
  aria-label="Search site"
  onclick="window.openSearch()"
>
  <svg width="18" height="18" viewBox="0 0 24 24" fill="none"
    stroke="currentColor" stroke-width="2" stroke-linecap="round">
    <circle cx="11" cy="11" r="8"></circle>
    <line x1="21" y1="21" x2="16.65" y2="16.65"></line>
  </svg>
  <span class="nav-search-label">Search</span>
</button>
```

Style in Nav.astro:
```css
.nav-search-btn {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  background: none;
  border: 1px solid currentColor;
  border-radius: 4px;
  padding: 0.3rem 0.6rem;
  cursor: pointer;
  font-size: 0.8rem;
  opacity: 0.7;
  transition: opacity 0.15s;
  color: inherit; /* inherits nav link color for both states */
}

.nav-search-btn:hover {
  opacity: 1;
}

.nav-search-label {
  font-size: 0.75rem;
  letter-spacing: 0.05em;
}

@media (max-width: 768px) {
  .nav-search-label {
    display: none; /* icon only on mobile */
  }
}
```

---

## STEP 5 — Import SearchModal in BaseLayout

In `BaseLayout.astro`, import and render the SearchModal component
just before the closing `</body>` tag:

```astro
---
import SearchModal from '../components/SearchModal.astro';
---

<!-- existing layout content -->

<SearchModal />
</body>
```

---

## STEP 6 — Build and Index

Run the full build:
```bash
npm run build
```

This runs `astro build` followed by `pagefind --site dist` automatically.
Pagefind will output something like:
```
Running Pagefind v1.x.x
Indexing...
Indexed N pages
Writing search index...
Done
```

The search index is written to `dist/pagefind/` and served as static files.

---

## VERIFICATION

- [ ] `npm run build` completes without errors
- [ ] Pagefind outputs "Indexed N pages" — N should match your article + hub page count
- [ ] Search icon visible in nav (desktop and mobile)
- [ ] Clicking search icon opens modal
- [ ] Pressing `/` opens modal
- [ ] Pressing `Cmd+K` / `Ctrl+K` opens modal
- [ ] Pressing `Esc` closes modal
- [ ] Clicking outside modal closes it
- [ ] Typing a search term returns results from actual articles
- [ ] Result links navigate to correct article pages
- [ ] Highlighted search terms visible in excerpts
- [ ] Modal colors match site palette (charcoal overlay, off-white modal, red accents)
- [ ] Mobile: search icon shows without label, modal fits screen
- [ ] `astro build` zero errors

---

## KNOWN UI FIXES (apply proactively — don't wait for review)

These two issues appeared on ilovecatalonia.com and should be fixed before deploying:

### 1. Input left padding
The Pagefind search icon sits inside the input field — default padding causes text to clash with it.
In the `#search-container` styles, set explicit left padding on the input:
```css
#search-container :global(.pagefind-ui__search-input) {
  padding-left: 2.75rem; /* accounts for the search icon */
}
```

### 2. Scrollable results + modal max-height
Without a max-height, the modal grows beyond the viewport on pages with many results.
Cap the modal and make the results container scroll independently:
```css
.search-modal {
  max-height: calc(100vh - 15vh);
  display: flex;
  flex-direction: column;
}

#search-container {
  overflow-y: auto;
  flex: 1;
}
```
Header (SEARCH label + close button) and footer (keyboard shortcuts) stay fixed.
Only the results list scrolls.

---

## NOTES

- The `dist/pagefind/` directory is generated at build time — do not commit it to git.
  Add to `.gitignore`:
  ```
  dist/
  ```
  Cloudflare Pages runs `npm run build` on deploy, so Pagefind re-indexes automatically.

- If articles are added later, just run `npm run build` again — index updates automatically.

- Pagefind respects `data-pagefind-ignore` — nav, footer, and related article cards
  are excluded so search results don't show duplicated content from sidebar widgets.
