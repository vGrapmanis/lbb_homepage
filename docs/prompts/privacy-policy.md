# Task: Create GDPR-compliant Privacy Policy for latvianbluesband.lv

## Context

This is the Latvian Blues Band website (see CLAUDE.md for full project context). The site is nearly complete — privacy policy is the final piece before launch. Domain is `.lv` (Latvia, EU jurisdiction), so **GDPR and Latvian Personal Data Processing Law apply**.

## What data the site actually collects

1. **Newsletter signup (live)** — Mailchimp embedded form in the Contact section. Collects: email address, signup timestamp, IP address, geolocation, engagement metrics (opens/clicks). Stored by Mailchimp (US-based, Intuit Inc.). This is the **only form on the site**.
2. **Google Analytics 4 (planned, not yet live)** — will collect: approximate location (city-level, IP-derived), device/browser info, page views, session data, traffic source. Uses cookies.
3. **Netlify hosting** — server logs (IP, user-agent) per standard hosting practice.
4. **No contact form** — booking uses a `mailto:` link, which opens the user's own email client (no data collected by us).
5. **Embeds** — Spotify and YouTube iframes set their own cookies when interacted with.

## Step 1: Research (do this before writing)

Use web search to verify current requirements. Don't rely on training data alone — GDPR guidance and Mailchimp/Google data-processing terms get updated. Specifically research:

1. **GDPR requirements for privacy policies in 2026** — mandatory disclosures under Articles 13/14
2. **Latvian-specific requirements** — Datu valsts inspekcija (DVI) guidance, Personas datu apstrādes likums
3. **Mailchimp as data processor** — current data processing terms, EU-US Data Privacy Framework status, what subprocessors they use
4. **Google Analytics 4 + GDPR** — current consensus on GA4 legality in EU (post-Schrems II, DPF status), IP anonymization defaults, consent requirements
5. **Cookie consent requirements** — does GA4 require a cookie banner under ePrivacy Directive? (Spoiler: yes for non-essential cookies)
6. **Netlify** — their DPA, where EU traffic is processed

## Step 2: Privacy Policy must cover

Structure the policy with these sections (use clear headings, plain language, no legal jargon where avoidable):

1. **Who we are** — data controller identity, contact email for privacy requests
2. **What data we collect** — split by source (newsletter, analytics, hosting logs, embeds)
3. **Why we collect it (legal basis)** — consent for newsletter, legitimate interest or consent for analytics, contract/legitimate interest for hosting
4. **Who we share it with** — Mailchimp, Google, Netlify, with links to their policies and notes on international transfers (US)
5. **International transfers** — EU-US Data Privacy Framework reliance, SCCs where applicable
6. **How long we keep it** — newsletter: until unsubscribe; analytics: GA4 default (recommend 14 months); logs: per Netlify
7. **Your rights under GDPR** — access, rectification, erasure, restriction, portability, objection, withdraw consent, lodge complaint with DVI (with link: https://www.dvi.gov.lv)
8. **Cookies** — what's set, by whom, purpose, how to opt out
9. **Children** — not directed at under-16s (GDPR threshold in Latvia is 13, but be conservative)
10. **Changes to this policy** — how updates are communicated
11. **Last updated** date

## Step 3: Implementation

1. Create the page as `src/pages/privacy.astro` using `BaseLayout.astro` for consistency.
2. The privacy policy is the **first exception to the single-page architecture** — it gets its own route. Keep nav working: clicking the site logo from `/privacy` should return to `/` (home).
3. Match the existing design system: use the color variables from `global.css`, the same typography (serif headings, sans body), generous spacing, readable line length (max-w-3xl or similar).
4. Add a small footer link to `/privacy` inside the Contact section's existing `Footer.astro` — don't add it to the main nav.
5. Add a sentence + checkbox (or clear consent text) near the Mailchimp newsletter input: "By subscribing you agree to our Privacy Policy" with link. Mailchimp's own double opt-in handles the formal consent record.
6. Generate **both Latvian and English versions** if feasible — primary audience is Latvian. If doing both, use `src/pages/privacy.astro` (English) and `src/pages/privatums.astro` (Latvian), with a language toggle at the top of each. If only one for now, **English first** (broader reach), Latvian version as a TODO.
7. Update `sitemap` config so `/privacy` is included.
8. Update `robots.txt` — `/privacy` should be indexable.

## Deliverables

1. The research summary (which sources you used, key findings, especially anything that changed recently)
2. `src/pages/privacy.astro` — full file
3. Any edits to `Footer.astro`, `NewsletterSignup.astro`, `astro.config.mjs` needed
4. A short note on what the **band owner must personally verify or fill in** before launch (e.g., the controller contact email, confirmation of GA4 retention settings, whether they've signed Mailchimp's DPA)

## Constraints

- No legal advice disclaimer needed in the policy itself, but flag in your summary that the owner should have a lawyer review before launch — this is a template grounded in current GDPR practice, not certified legal counsel.
- Plain language. Real humans should be able to read it.
- Don't pad with boilerplate that doesn't apply (no "we may collect your social security number" template junk).
- Match site tone: clear, grown-up, no corporate hedging.
