# Travel Guide Network — Project Setup Guide
v2.0 · February 2026

Complete setup for the Claude Project that manages the entire site network.
All canonical files live in GitHub — no re-uploading, always current.

---

## PART 1 — STEP BY STEP SETUP

### Step 1 — Create the GitHub Repo

1. Go to github.com → New repository
2. Name it: `travel-guide-network`
3. Visibility: **Public** (required so Claude can fetch raw file URLs)
4. Initialize with a README: No (we have our own)
5. Create repository

Upload all files from this session maintaining the structure:
```
travel-guide-network/
├── README.md
├── playbook/
│   └── playbook.md
├── prompts/
│   ├── PROMPT-1-base-build.md
│   ├── PROMPT-2-articles.md
│   ├── PROMPT-3-ui-fixes.md
│   ├── PROMPT-4-seo-infrastructure.md
│   └── PROMPT-5-required-pages.md
└── site-briefs/
    ├── SITE-TEMPLATE.md
    └── SITE-ilovecatalonia.md
```

---

### Step 2 — Get the Raw URLs

Every file is fetchable at:
```
https://raw.githubusercontent.com/[YOUR-USERNAME]/travel-guide-network/main/[path]
```

Key URLs to save (replace [YOUR-USERNAME]):
```
PLAYBOOK:
https://raw.githubusercontent.com/[YOUR-USERNAME]/travel-guide-network/main/playbook/playbook.md

PROMPT-1:
https://raw.githubusercontent.com/[YOUR-USERNAME]/travel-guide-network/main/prompts/PROMPT-1-base-build.md

PROMPT-2:
https://raw.githubusercontent.com/[YOUR-USERNAME]/travel-guide-network/main/prompts/PROMPT-2-articles.md

PROMPT-3:
https://raw.githubusercontent.com/[YOUR-USERNAME]/travel-guide-network/main/prompts/PROMPT-3-ui-fixes.md

PROMPT-4:
https://raw.githubusercontent.com/[YOUR-USERNAME]/travel-guide-network/main/prompts/PROMPT-4-seo-infrastructure.md

PROMPT-5:
https://raw.githubusercontent.com/[YOUR-USERNAME]/travel-guide-network/main/prompts/PROMPT-5-required-pages.md

SITE BRIEF — ilovecatalonia:
https://raw.githubusercontent.com/[YOUR-USERNAME]/travel-guide-network/main/site-briefs/SITE-ilovecatalonia.md

SITE TEMPLATE:
https://raw.githubusercontent.com/[YOUR-USERNAME]/travel-guide-network/main/site-briefs/SITE-TEMPLATE.md
```

---

### Step 3 — Create the Claude Project

1. Go to claude.ai → Projects → New Project
2. Name it: `Travel Guide Network`
3. Add the system prompt from Part 2 below (with your actual GitHub username filled in)
4. Upload only `SYSTEM.md` to Project Knowledge — everything else is fetched live from GitHub

---

### Step 4 — How Claude Fetches Files On Demand

In any Claude Project conversation, you can say:

> "Fetch the base build prompt and adapt it for VisitProsecco.com"

Claude will fetch the raw URL, read the current content, fill in the variables, and output a ready-to-paste Claude Code prompt. If you've updated the file on GitHub, Claude gets the latest version automatically — no re-uploading.

You can also be explicit:
> "Fetch https://raw.githubusercontent.com/[username]/travel-guide-network/main/prompts/PROMPT-1-base-build.md"

---

### Step 5 — Updating Files

When a prompt or the playbook needs updating:
1. Edit the file directly on GitHub (click the pencil icon on any file)
2. Commit the change
3. Done — Claude will fetch the updated version on next request

No downloading, no re-uploading to Claude Project.

---

### Step 6 — Claude Code Workflow

Claude Code is separate from the Claude Project.
- **Claude Project** = strategy, planning, generating adapted prompts
- **Claude Code** = actually building and editing site files

```
CLAUDE PROJECT (claude.ai)          CLAUDE CODE (terminal)
───────────────────────────         ──────────────────────
Fetches prompt from GitHub    →     Paste prompt → runs build
Adapts variables for site     →     astro build → git push
Reviews screenshots           →     UI fixes → git push
Generates next prompt         →     Paste → repeat
```

Install Claude Code:
```bash
npm install -g @anthropic-ai/claude-code
```

In your site repo:
```bash
cd ~/Sites/ilovecatalonia
claude
```

---

### Step 7 — Starting a New Site

1. **In Claude Project:** "Starting VisitProsecco.com. Fetch PROMPT-1 and adapt it."
   Claude fetches from GitHub, fills in variables, outputs ready-to-paste prompt.

2. **In terminal:**
   ```bash
   mkdir visitprosecco && cd visitprosecco
   git init
   claude
   ```

3. Paste PROMPT-1 → deploy → visual review

4. **In Claude Project:** "Fetch PROMPT-2 and adapt for VisitProsecco." → paste → run

5. **In Claude Project:** "Fetch PROMPT-3 and adapt." → paste after visual review

6. **In Claude Project:** "Fetch PROMPT-4 and adapt." → paste

7. **In Claude Project:** "Fetch PROMPT-5 and adapt." → paste

8. Apply for AdSense. Set up Google CMP.

9. **In Claude Project:** Create SITE-visitprosecco.md → commit to GitHub

---

## PART 2 — SYSTEM PROMPT
**Paste this into: Claude.ai → Project Instructions**
*(Replace [YOUR-USERNAME] with your actual GitHub username)*

---

You are the operations assistant for a network of independent regional travel and culture guide websites. Your role is to help plan, build, maintain, and grow each site in the network efficiently.

**All canonical files live on GitHub. Fetch them on demand — do not rely on memory.**

Base URL: `https://raw.githubusercontent.com/[YOUR-USERNAME]/travel-guide-network/main/`

Files available:
- `playbook/playbook.md` — full operations playbook. Read before any strategic recommendation.
- `prompts/PROMPT-1-base-build.md` — full Astro site from scratch
- `prompts/PROMPT-2-articles.md` — 5 articles + FAQ + schema
- `prompts/PROMPT-3-ui-fixes.md` — all visual fixes
- `prompts/PROMPT-4-seo-infrastructure.md` — schema sprint + OG images + AdSense
- `prompts/PROMPT-5-required-pages.md` — About, Contact, Privacy, Terms, Disclaimer
- `site-briefs/SITE-ilovecatalonia.md` — ilovecatalonia.com brief (active, live)
- `site-briefs/SITE-TEMPLATE.md` — blank template for new sites

**When generating Claude Code prompts:**
Always fetch the relevant PROMPT file first. Never write a prompt from scratch.
Swap the site-specific variables at the top. Keep the structure identical.

**When working on a specific site:**
Fetch that site's brief first. Do not make recommendations that contradict what's already been decided.

**When writing FAQ content:**
Research first. Ask which article needs a FAQ, then look for real traveler questions before writing. Do not invent generic questions.

**When making SEO decisions:**
Follow the keyword framework in the playbook. Prioritize under-served long-tail queries over competitive head terms.

**Tool distinctions — always be explicit:**
- Decisions, writing, prompts, strategy → handled here in Claude Project
- Actually building or editing site files → Claude Code (separate terminal tool)
- Deploying → git push → auto-deploys via Cloudflare Pages

**Tone:** Direct and results-oriented. No padding. If something isn't working, say so.

**The network model:** Each site shares the same Astro codebase and SEO framework. Lessons learned from one site go into the playbook on GitHub.

---

## PART 3 — QUICK REFERENCE CARD

```
TOOL            USE FOR                           NOT FOR
──────────────  ────────────────────────────────  ──────────────────────
Claude Project  Strategy, SEO, content,           Building site files,
(claude.ai)     adapted prompts, site briefs,     running code,
                fetching files from GitHub        deploying

Claude Code     Building Astro site, editing      Strategy, content
(terminal)      files, running astro build,       writing, SEO planning
                UI fixes, deploying via git push

GitHub          Canonical prompt files,           Direct site editing
(public repo)   playbook, site briefs —
                fetched live by Claude

Cloudflare      Hosting + CDN,                    Site building
Pages           auto-deploy from GitHub
```

**The typical workflow for any task:**
1. Open Claude Project → describe what you need
2. Claude fetches relevant file from GitHub → adapts for the site
3. Copy prompt → paste into Claude Code in site repo
4. Run `astro build` to verify → `git push` to deploy
5. Check deployed URL → screenshot issues → share in Claude Project
6. Claude fetches next prompt → repeat
