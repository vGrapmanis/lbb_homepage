# Codebase Optimization Audit & Refactor

> Use this prompt when the codebase needs a holistic sweep for **readability, performance, and security**. This is an auditing + refactoring task, not a feature task. Work in phases. Do not skip the audit.

---

## Required Reading (do this first, every time)

1. Read `CLAUDE.md` end-to-end.
2. Read every file in `docs/` (use the docs reference table in CLAUDE.md to understand what each covers).
3. List the actual project structure (`src/`, `public/`, root configs).
4. Only then start auditing.

If anything in this prompt conflicts with `CLAUDE.md` or `docs/`, **`CLAUDE.md` wins** — surface the conflict in your audit report and ask before resolving it.

---

## Goals (in priority order)

1. **Transparency** — the codebase should be readable, predictable, and easy for a new contributor (or future me) to navigate. No mystery, no cramped style blocks, no 400-line components.
2. **Performance** — fast first paint, low JS payload, no layout shift, embeds lazy-loaded, images optimized. Target: Lighthouse 90+ across the board.
3. **Security** — proper response headers, no leaked secrets, safe external links, locked-down iframes.
4. **Preserve all existing functionality and visual output.** This is a refactor, not a redesign. If a change is visible to the user, flag it before doing it.

---

## Methodology (mandatory, in this order)

### Phase 1 — Audit (read-only)

Walk the entire codebase. Do **not** edit anything yet. Produce a written audit report covering every category in the checklist below. For each finding, include:

- **File and line numbers**
- **What's wrong** (one sentence)
- **Why it matters** (transparency / performance / security)
- **Proposed fix** (one sentence)
- **Risk level** (low / medium / high — does this touch visible behavior?)

Group findings by category, then by severity. Be honest — if something is already fine, say so and move on. Don't invent problems to justify the audit.

### Phase 2 — Plan

Based on the audit, propose an ordered refactor plan:

- Group related changes into logical commits (one concern per commit).
- Order commits low-risk → high-risk.
- Call out any change that requires a design decision from me (e.g. "should we extract these into a tokens file?") and **stop and ask** before doing those.
- Estimate roughly how many files each commit touches.

**Wait for me to approve the plan before touching any code.**

### Phase 3 — Execute

Work one commit at a time. After each commit:

- `npm run build` must succeed.
- `npm run dev` must render every section without visual regressions.
- Show a short summary of what changed and why.
- Move to the next commit only after I confirm.

---

## Audit Checklist

### 1. File Organization & Transparency

- [ ] Are any `.astro` components over ~150 lines? Split them into smaller, single-purpose components.
- [ ] Is there a `<style>` block over ~30 lines inside a component? Consider whether it belongs in `src/styles/global.css`, a dedicated component stylesheet, or stays scoped — and justify the choice.
- [ ] Are CSS properties cramped onto few lines, grouped illogically, or missing whitespace between rule blocks? Reformat with consistent indentation, logical property groupings (positioning → box model → typography → visual → transitions), and one blank line between rule blocks.
- [ ] Are there repeated patterns (markup or styles) that should be extracted into a component, a utility, or a CSS variable?
- [ ] Are `members.json`, `shows.json`, `discography.json` doing only data — no presentation logic leaking in?
- [ ] Are file and component names consistent with the conventions in `CLAUDE.md` (PascalCase components, camelCase data)?
- [ ] Is there dead code — unused imports, unreferenced components, commented-out blocks, leftover console.logs?
- [ ] Does every non-obvious block have a short comment explaining *why* (not *what*)?
- [ ] Are magic numbers (px values, durations, z-indexes) replaced with CSS variables or Tailwind tokens?

### 2. CSS Quality

- [ ] Is every color, spacing primitive, font, and animation timing referenced through a CSS variable from `:root`? No hardcoded `#C4842D` scattered through components.
- [ ] Are long Tailwind class strings (>~8 utilities repeated in multiple places) extracted into a component or an `@apply` rule?
- [ ] Are there duplicate or conflicting rules across `global.css` and component `<style>` blocks?
- [ ] Is `!important` used anywhere? Justify each occurrence or remove it.
- [ ] Are vendor prefixes still present where modern browsers no longer need them (e.g. `-webkit-transition`)?
- [ ] Is `prefers-reduced-motion` respected? Wrap scroll-reveal, hover scale/lift, and any non-essential animation in a `@media (prefers-reduced-motion: no-preference)` block so users with reduced-motion settings get a calm experience.
- [ ] Are hover-only effects scoped with `@media (hover: hover)` per the responsive section in `CLAUDE.md`?

### 3. Performance

- [ ] Do all embeds (Spotify, YouTube, anything else) have `loading="lazy"`?
- [ ] Are YouTube embeds using a lightweight facade pattern (poster image + click-to-load iframe) for the ones below the fold? The full iframe is heavy — only the visible/primary one should load eagerly.
- [ ] Are images served through Astro's `<Image>` component with explicit `width` and `height` (prevents CLS) and modern formats (WebP/AVIF)?
- [ ] Is the hero image preloaded (`<link rel="preload" as="image">`) and marked `loading="eager"` + `fetchpriority="high"`?
- [ ] Are all other images `loading="lazy"` and `decoding="async"`?
- [ ] Are Google Fonts loaded with `display=swap` and preconnected (`<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>`)? Only load the weights actually used.
- [ ] Is the GA4 script `async` and placed so it doesn't block render?
- [ ] Is JavaScript shipped to the client minimal? Astro should ship zero JS by default — flag any component using `client:*` directives and confirm each is necessary (mobile menu, intersection observer, GA4 events are the expected exceptions).
- [ ] Is the Intersection Observer registered once globally rather than per-component, and does it disconnect once an element has revealed?
- [ ] Is Tailwind purging production CSS correctly (`tailwind.config.mjs` `content` paths cover every file using utilities)?
- [ ] Are there any large dependencies in `package.json` that aren't actually used? Run `npm ls` and check.
- [ ] Is the scroll-snap container causing layout thrashing on mobile? Verify `min-height: 100dvh` is used and `scroll-snap-type: y proximity` is considered for overflow-prone sections.
- [ ] Does the build output (`dist/`) have any unexpectedly large files? Inspect `dist/_astro/` for bloated CSS or JS chunks.

### 4. Security

- [ ] Does `netlify.toml` set the following response headers (add if missing):
  - `Content-Security-Policy` — strict policy allowlisting only required sources: self, Spotify (`*.spotify.com`, `*.scdn.co`), YouTube (`*.youtube.com`, `*.ytimg.com`, `*.youtube-nocookie.com`), Mailchimp (`*.list-manage.com`), Google Fonts (`fonts.googleapis.com`, `fonts.gstatic.com`), GA4 (`*.googletagmanager.com`, `*.google-analytics.com`). Use `frame-src` for iframes, `script-src` for scripts, `connect-src` for fetch/beacon.
  - `Strict-Transport-Security: max-age=31536000; includeSubDomains; preload`
  - `X-Content-Type-Options: nosniff`
  - `X-Frame-Options: DENY` (or `SAMEORIGIN` if needed)
  - `Referrer-Policy: strict-origin-when-cross-origin`
  - `Permissions-Policy` — disable features not used (geolocation, microphone, camera, etc.)
- [ ] Do all external `<a>` links have `rel="noopener noreferrer"` and `target="_blank"` where appropriate? Social links, ticket links, streaming links — all of them.
- [ ] Do iframes have a `title` attribute (accessibility) and consider `sandbox` where the embed allows it?
- [ ] Are there any secrets, API keys, or tokens committed to the repo? The GA4 Measurement ID and Mailchimp `USER_ID`/`LIST_ID` are public by design — that's fine. Anything else is not.
- [ ] Is the Mailchimp honeypot field actually present in `NewsletterSignup.astro` per the spec in `CLAUDE.md`?
- [ ] Is `.env` (if it exists) in `.gitignore`? Is there any `.env.example` that accidentally contains real values?
- [ ] Are dependencies up to date? Run `npm audit` and report findings. Don't auto-fix — flag and let me decide.
- [ ] If any third-party script is loaded from a CDN (not Google's GA4), does it use Subresource Integrity (`integrity` attribute)?

### 5. Accessibility (this is a transparency issue too — semantic HTML reads itself)

- [ ] Is the page structure semantic: `<header>` for nav, `<main>` for content, `<section>` for each scroll section (with `aria-label` or `aria-labelledby`), `<footer>` for footer?
- [ ] Does every image have a meaningful `alt` (or `alt=""` if purely decorative)?
- [ ] Do icon-only buttons and links (social icons, hamburger) have `aria-label`?
- [ ] Is there a visible `:focus-visible` style on every interactive element? Tab through the whole page and verify.
- [ ] Is there a skip-to-content link for keyboard users?
- [ ] Does the mobile drawer trap focus when open and restore focus to the trigger when closed?
- [ ] Does the active-section nav indicator use `aria-current="page"` (or `aria-current="true"`)?
- [ ] Do color combinations meet WCAG AA contrast? Verify the amber accent on the deep-bg colors with a contrast checker — the palette in `CLAUDE.md` should be checked, not assumed.

### 6. SEO Verification

- [ ] Are all meta tags from `CLAUDE.md`'s SEO section actually present in `BaseLayout.astro`?
- [ ] Is the JSON-LD `MusicGroup` structured data present and valid? Validate at `validator.schema.org`.
- [ ] Is `astro.config.mjs` setting `site: 'https://latvianbluesband.lv'`?
- [ ] Is `@astrojs/sitemap` configured and outputting a sitemap?
- [ ] Is `robots.txt` present and correct?
- [ ] Does every heading hierarchy make sense (one `<h1>` per page, no skipped levels)?

### 7. Astro-Specific Best Practices

- [ ] Are component scripts (frontmatter `---`) doing only what's needed at build time? No client-side logic snuck in.
- [ ] Are static imports preferred over dynamic where possible?
- [ ] Is the `BaseLayout` slot pattern used consistently rather than duplicated layout markup?
- [ ] Are partials small and composable? `Navbar`, `Footer`, `HeroBanner`, `SpotifyPlayer`, `YouTubeEmbed`, `DiscographyCard`, `ShowCard`, `MemberCard`, `NewsletterSignup`, `SectionWrapper` should each have one clear responsibility.

---

## Deliverable Format

Produce the audit as a single markdown file (or inline reply if short). Structure:

```
# Audit Report — [date]

## Summary
- N findings across M files
- Highest-priority issues: [bulleted list]
- Overall codebase health: [one paragraph honest assessment]

## Findings by Category
### File Organization
- [file:line] Issue. Why it matters. Fix. (risk: low/med/high)
...

### CSS Quality
...

[continue for each category]

## Proposed Refactor Plan
Commit 1: [name] — [files] — [risk]
Commit 2: ...
...

## Open Questions for the Owner
- [things needing my decision before proceeding]
```

---

## Constraints (do not violate)

- **Do not change visible design, copy, layout, or behavior** without flagging it first.
- **Do not introduce new dependencies** without justification and approval. The stack is locked: Astro, Tailwind, vanilla JS. No animation libraries, no UI kits, no utility packages.
- **Do not ship more JavaScript to the client.** If anything, ship less. Astro's static output is the point.
- **Do not rewrite `CLAUDE.md` or `docs/`** unless I ask. If the audit reveals the docs are wrong, flag it — don't silently edit.
- **Do not commit anything yet.** Every commit happens after I approve the plan and confirm each step.
- **Do not run destructive commands** (`rm -rf`, `git reset --hard`, force-push) without explicit confirmation.
- If you're uncertain whether a change is in scope, **ask** rather than assume.

---

## Final Note

The goal here is a codebase I can read at 11pm without coffee and understand in five minutes. Every file should have one job. Every style block should be skimmable. Every external interaction should be safe. Optimize for clarity first — the performance and security wins will mostly follow.
