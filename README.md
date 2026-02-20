# Travel Guide Network

Canonical files for building and operating an independent regional travel guide network.
Based on [ilovecatalonia.com](https://ilovecatalonia.com).

---

## How to Use

All files are fetchable via raw GitHub URLs. In any Claude session, paste:

> "Fetch [raw URL] and use it as the prompt for this task."

Raw URL format:
```
https://raw.githubusercontent.com/[YOUR-USERNAME]/travel-guide-network/main/[path/to/file.md]
```

---

## Repo Structure

```
travel-guide-network/
├── README.md
├── playbook/
│   └── playbook.md                    ← operations playbook (v3)
├── prompts/
│   ├── PROMPT-1-base-build.md         ← full Astro site from scratch
│   ├── PROMPT-2-articles.md           ← 5 articles + FAQ + schema
│   ├── PROMPT-3-ui-fixes.md           ← all visual fixes
│   ├── PROMPT-4-seo-infrastructure.md ← schema sprint + OG + AdSense
│   └── PROMPT-5-required-pages.md     ← About, Contact, Privacy, Terms, Disclaimer
└── site-briefs/
    ├── SITE-TEMPLATE.md               ← blank template for new sites
    └── SITE-ilovecatalonia.md         ← ilovecatalonia.com (active)
```

---

## The 5-Prompt Build Sequence

| # | File | When to Run | What It Builds |
|---|------|-------------|----------------|
| 1 | PROMPT-1-base-build.md | Blank repo — first | Full Astro site, all hub pages, infrastructure |
| 2 | PROMPT-2-articles.md | After base build | 5 articles, FAQ, schema verification |
| 3 | PROMPT-3-ui-fixes.md | After visual review | Logo, nav, hero, TOC, cards, breadcrumbs |
| 4 | PROMPT-4-seo-infrastructure.md | After UI fixes | Schema, OG images, WebSite schema, AdSense |
| 5 | PROMPT-5-required-pages.md | Before AdSense | About, Contact, Privacy, Terms, Disclaimer |

Update the `VARIABLES` block at the top of each prompt before running.

---

## After All 5 Prompts

1. Submit sitemap in Google Search Console
2. Apply for AdSense at adsense.google.com
3. Set up Google CMP consent banner (EEA)
4. Request re-indexing for homepage in GSC
5. Set www → non-www 301 redirect in Cloudflare dashboard
6. Run Rich Results Test on 1 article + 1 hub page

---

## Site Network

| Domain | Region | Status |
|--------|--------|--------|
| [ilovecatalonia.com](https://ilovecatalonia.com) | Catalonia, Spain | Live — AdSense pending |
| VisitProsecco.com | Prosecco Hills, Veneto | Planning |

---

## Cloudflare Setup (Manual — Not in Prompts)

- GitHub → Cloudflare Pages: build command = `astro build`, output = `dist`
- Custom domain: Pages → Custom domains
- www redirect: 301 permanent to non-www (not 302)
- HTTPS: automatic via Cloudflare
