# PROMPT-5-required-pages.md
# Regional Travel Guide Network — Required Pages (AdSense + E-E-A-T)
# Creates 5 pages required for AdSense approval and Google trust signals.
# Run after base build. Adapt content variables before running.

---

## VARIABLES — UPDATE BEFORE RUNNING

```
SITE_NAME        = "ilovecatalonia.com"
SITE_URL         = "https://ilovecatalonia.com"
REGION_SHORT     = "Catalonia"
AUTHOR_NAME      = "Franck"
CONTACT_EMAIL    = "hola@ilovecatalonia.com"
AUTHOR_BIO       = "digital marketer and domain investor based in Paris"
AUTHOR_CONNECTION = "been visiting Catalonia long enough to have opinions about it that nobody asked for"
SITE_TAGLINE     = "Not affiliated with the Catalan Tourism Board — just obsessed with the place."
TOURISM_BOARD    = "Catalan Tourism Board"
GOVERNING_LAW    = "applicable law"   # or "French law" / "Spanish law"
```

---

## INSTRUCTIONS

Create 5 pages using BaseLayout.astro.
Match site typography, spacing, and color tokens.
Plain prose layout — no new components needed.
Add all 5 pages to footer Legal column.
Run `astro build` after all pages are created.

---

## PAGE 1 — /about/

File: `src/pages/about.astro`
Title: "About [SITE_NAME]"
Meta: "An independent travel and culture guide to [REGION_SHORT], written by [AUTHOR_NAME] — [AUTHOR_BIO]."

Add Person schema to `<head>`:
```json
{
  "@context": "https://schema.org",
  "@type": "Person",
  "name": "[AUTHOR_NAME]",
  "url": "[SITE_URL]/about/",
  "worksFor": { "@type": "Organization", "name": "[SITE_NAME]", "url": "[SITE_URL]" }
}
```

Content template (adapt tone to match site articles — first person, specific):

---
My name is [AUTHOR_NAME]. I'm a [AUTHOR_BIO], and I've [AUTHOR_CONNECTION].

This site exists because the travel content about [REGION_SHORT] is, with a few exceptions, terrible. [Adapt: tourism board brochures, listicles, AI-generated content — none of them produce what travelers actually need.]

So I built this instead.

**What you'll find here**

[2–3 paragraphs on what the site covers and why it's different.
Be specific. Name actual content. Reference the editorial voice.]

**What this site is not**

[SITE_NAME] is not affiliated with the [TOURISM_BOARD] or any official tourism body. No destination board has paid for coverage. When affiliate relationships and sponsorships are part of the content, it will be disclosed clearly on every relevant page.

This is an independent editorial site. The independence is the point.

**Get in touch**

[Contact email as mailto link]
---

Also add author byline to ArticleLayout.astro if not already present:
Below article title: "By [AUTHOR_NAME] · [X] min read"
Link [AUTHOR_NAME] to /about/

---

## PAGE 2 — /contact/

File: `src/pages/contact.astro`
Title: "Contact — [SITE_NAME]"
Meta: "Get in touch with [SITE_NAME] — corrections, local knowledge, and collaboration enquiries."

Content:

---
# Contact

**Email:** [CONTACT_EMAIL as mailto link]

Happy to hear from you about:
- **Corrections** — if something is wrong or outdated, please tell me
- **Local knowledge** — if you know the region and something important is missing
- **Collaboration** — press trips, sponsored content, partnerships (read the Disclaimer first)
- **General** — anything else

Response time: typically 2–3 working days.
---

---

## PAGE 3 — /privacy/

File: `src/pages/privacy.astro`
Title: "Privacy Policy — [SITE_NAME]"
Meta: "Privacy policy for [SITE_NAME] — analytics, advertising, and data handling."

CRITICAL: Must include all sections below for GDPR + AdSense compliance.

Content:

---
# Privacy Policy

*Last updated: [current month year]*

## Who we are

[SITE_NAME] is an independent travel guide to [REGION_SHORT].
Data controller: [AUTHOR_NAME], [CONTACT_EMAIL]

## Analytics

This site uses Plausible Analytics — a privacy-first tool that collects
no personal data, sets no cookies, and does not track visitors across sites.
No consent banner is required for EU visitors.

## Advertising

If display advertising is active on this site, third-party vendors including
Google may use cookies to serve ads based on your prior visits to this or
other websites. You can opt out of personalised advertising at
google.com/settings/ads.

## Affiliate links

Some links on this site may be affiliate links. If you book or purchase
through them, this site may earn a small commission at no extra cost to you.
See the Disclaimer for full details.

## Cookies

This site sets no cookies through its own code. Third-party services
(analytics, advertising if active) may set their own cookies.

## Your rights (GDPR)

EU residents have the right to access, correct, or request deletion of
personal data. Contact: [CONTACT_EMAIL]. Response within 30 days.
As this site uses cookieless analytics, there is unlikely to be
personal data to action.

## External links

We are not responsible for the privacy practices of external sites.

## Changes

This policy may be updated periodically. The date above reflects the
current version.
---

---

## PAGE 4 — /terms/

File: `src/pages/terms.astro`
Title: "Terms of Use — [SITE_NAME]"
Meta: "Terms of use for [SITE_NAME]."

Content:

---
# Terms of Use

*Last updated: [current month year]*

## Use of this site

[SITE_NAME] provides travel and cultural information for general
informational purposes. By using this site you accept these terms.

## Accuracy

Travel information changes. Festival dates, prices, timetables, and
opening hours are accurate at time of writing but may change. Always
verify critical details directly with providers before travelling.
[SITE_NAME] accepts no liability for loss arising from reliance on
information published here.

## Intellectual property

All original content on [SITE_NAME] — text, structure, presentation —
is © [current year] [SITE_NAME]. All rights reserved. Short quotations
for non-commercial purposes with attribution and a link are permitted.
Reproduction of substantial portions without permission is not.

## External links

Links to external sites do not constitute endorsement. [SITE_NAME]
is not responsible for the content or availability of external sites.

## Affiliate relationships

Some links are affiliate links. See the Disclaimer for full disclosure.

## Limitation of liability

To the fullest extent permitted by law, [SITE_NAME] shall not be liable
for any loss arising from use of this site or reliance on its content.

## Governing law

These terms are governed by [GOVERNING_LAW].

## Changes

Terms may be updated at any time. Continued use constitutes acceptance.

## Contact

[CONTACT_EMAIL as mailto link]
---

---

## PAGE 5 — /disclaimer/

File: `src/pages/disclaimer.astro`
Title: "Disclaimer — [SITE_NAME]"
Meta: "Editorial independence, affiliate disclosure, and sponsored content policy for [SITE_NAME]."

Content:

---
# Disclaimer

*Last updated: [current month year]*

## Editorial independence

[SITE_NAME] is not affiliated with the [TOURISM_BOARD] or any
official tourism or government body. Editorial decisions are made
independently. No brand has paid for editorial coverage unless
explicitly disclosed as sponsored content.

## Affiliate links

Some links on this site are affiliate links. If you click through and
make a booking or purchase, [SITE_NAME] may earn a small commission —
at no extra cost to you. Current affiliate relationships include booking
platforms and tour operators in the travel sector. Commission never
influences editorial recommendations.

## Sponsored content

Any content produced in partnership with a brand or tourism body is
clearly labeled **"In partnership with [X]"** at the top of the page.
Sponsored content is held to the same factual standards as editorial content.

## Accuracy

Information is researched and verified at time of publication. Travel
details change — always confirm with providers before travelling.

## Contact

[CONTACT_EMAIL as mailto link]
---

---

## FOOTER UPDATE

In Footer.astro, the Legal column must link to all 5 pages:
```
LEGAL
About
Contact
Privacy Policy
Terms of Use
Disclaimer
```

Style: same as Explore column — uppercase bold heading, muted link list below.
This is the 4th column. Do NOT change the existing 3 columns.

---

## VERIFICATION

- [ ] /about/ — loads, author name visible, Person schema in `<head>`
- [ ] /contact/ — loads, email is a working mailto link
- [ ] /privacy/ — loads, contains AdSense cookie disclosure
- [ ] /terms/ — loads
- [ ] /disclaimer/ — loads, affiliate disclosure present
- [ ] Footer: Legal column visible with all 5 links
- [ ] Author byline on all articles links to /about/
- [ ] astro build: zero errors
