# CLAUDE.md — Latvian Blues Band Homepage

## Project Overview

Official homepage for Latvian Blues Band (latvianbluesband.lv).
A **single-page scrolling website** with six full-viewport sections: Home, Media, Discography, Tour, About, Contact.
The **Home section** is a full-viewport poster slideshow (`PosterSlideshow.astro`) — auto-advances every 5 s, left/right arrows on desktop, swipe on mobile, dot indicators. Posters loaded from `public/images/posters/` at build time, sorted by filename (YYYY-MM-DD).
Each section fills the entire screen. Scrolling snaps from one section to the next.
Navigation bar stays fixed on top at all times.
Goal: professional, atmospheric, memorable — a site that feels like the blues sounds.

---

## Tech Stack

| Layer      | Choice                          | Why                                                     |
| ---------- | ------------------------------- | ------------------------------------------------------- |
| Framework  | **Astro**                       | Static HTML output, component-based, fast, SEO-friendly |
| Styling    | **Tailwind CSS**                | Utility-first, responsive out of the box                |
| Animations | **CSS transitions + keyframes** | Zero extra dependencies                                 |
| Newsletter | **Mailchimp**                   | Free tier (500 subs), subscriber metadata, remarketing  |
| Analytics  | **Google Analytics 4**          | Traffic tracking, audience insights, event tracking     |
| Hosting    | **FTP (FileZilla)**             | Current: manual upload of `dist/` to shared hosting via FileZilla FTP |
| Repo       | **GitHub**                      | Source control; future migration to Netlify planned     |

### Why Astro?

- Outputs pure static HTML/CSS/JS — no framework shipped to browser
- Component-based — reusable NavBar, Footer, MemberCard, etc.
- Single `index.astro` page with section components = clean single-page scroll architecture
- Built-in image optimization, markdown support

### Why NOT Netlify Forms?

- No contact form — users email directly via `mailto:` link
- Newsletter handled entirely by Mailchimp

---

## Single-Page Scroll Architecture

One `index.astro` page containing six full-viewport sections stacked vertically.

```
┌──────────────────────────────┐
│  NAVBAR (fixed, always on top)│  ← z-index: 50, position: fixed
├──────────────────────────────┤
│     SECTION: HOME            │  ← id="home"
│     SECTION: MEDIA           │  ← id="media"
│     SECTION: DISCOGRAPHY     │  ← id="discography"
│     SECTION: TOUR            │  ← id="tour"
│     SECTION: ABOUT           │  ← id="about"
│     SECTION: CONTACT         │  ← id="contact"
└──────────────────────────────┘
Each section: min-height: 100vh, scroll-snap-align: start
```

### Scroll-Snap CSS

```css
html {
  scroll-snap-type: y mandatory;
  scroll-behavior: smooth;
}
.section {
  min-height: 100vh;
  scroll-snap-align: start;
  scroll-snap-stop: always;
}
```

### Navigation Behavior

- Nav links are anchor links: `#home`, `#media`, `#discography`, `#tour`, `#about`, `#contact`
- Intersection Observer detects active section → highlights corresponding nav link
- Nav background: transparent over hero, transitions to solid when scrolled past first section
- Mobile: hamburger menu with slide-in drawer, links close drawer after click

---

## Site Architecture

```
latvian-blues-band/
├── public/
│   ├── images/
│   │   ├── band/              # Band photos, banner images
│   │   ├── members/           # Individual member headshots
│   │   ├── albums/            # Album cover art
│   │   ├── posters/           # Event posters — named YYYY-MM-DD.jpeg, sorted by date
│   │   └── logo.svg           # Band logo
│   ├── fonts/
│   ├── robots.txt
│   └── favicon.ico
├── src/
│   ├── components/
│   │   ├── Navbar.astro
│   │   ├── Footer.astro
│   │   ├── PosterSlideshow.astro  # Home section — event poster carousel
│   │   ├── HeroBanner.astro       # Unused (replaced by PosterSlideshow)
│   │   ├── SpotifyPlayer.astro
│   │   ├── YouTubeEmbed.astro
│   │   ├── DiscographyCard.astro
│   │   ├── ShowCard.astro
│   │   ├── MemberCard.astro
│   │   ├── NewsletterSignup.astro
│   │   └── SectionWrapper.astro
│   ├── layouts/
│   │   └── BaseLayout.astro
│   ├── pages/
│   │   └── index.astro        # THE SINGLE PAGE — assembles all sections
│   ├── data/
│   │   ├── shows.json
│   │   ├── members.json
│   │   └── discography.json
│   └── styles/
│       └── global.css
├── docs/                      # Detailed specs (see below)
├── astro.config.mjs
├── tailwind.config.mjs
├── package.json
├── netlify.toml
└── CLAUDE.md
```

---

## Detailed Specs

Detailed specs are in `/docs/`. Read the relevant doc before working on that area.

| File                        | Contents                                              |
| --------------------------- | ----------------------------------------------------- |
| `docs/sections.md`          | All six section specifications + navbar               |
| `docs/design.md`            | Colors, typography, animations, visual effects        |
| `docs/responsive.md`        | Breakpoints, mobile rules, scroll-snap, touch targets |
| `docs/integrations.md`      | Mailchimp, GA4, Spotify/YouTube embeds, SEO           |
| `docs/deployment.md`        | Netlify setup, DNS migration, domain guide            |
| `docs/content-checklist.md` | Assets the owner must provide before launch           |
| `docs/data-schemas.md`      | JSON structures for shows, members, discography       |

---

## Coding Conventions

- **Components:** PascalCase filenames (`MemberCard.astro`)
- **Data files:** camelCase filenames (`shows.json`)
- **CSS:** Tailwind utilities first, `global.css` for theme variables
- **Animations:** CSS-only where possible. JS only for Intersection Observer and scroll tracking
- **Images:** WebP preferred, fallback JPG. Use Astro `<Image>` for optimization
- **Accessibility:** Every image has `alt`. Interactive elements have `aria-labels`. Touch targets min 44x44px
- **Performance:** All embeds `loading="lazy"`. No unnecessary JS. Lighthouse 90+
- **Commits:** Conventional — `feat: add hero banner`, `fix: mobile nav toggle`

---

## Key Commands

```bash
npm run dev          # Start dev server (localhost:4321)
npm run build        # Build for production (output in dist/)
npm run preview      # Preview production build locally
```

---

## Important Constraints

- **No paid services** — free tiers only (Netlify, Mailchimp, GA4, Google Fonts)
- **No backend/server** — static site only
- **No contact form** — email via mailto: link
- **No client-side framework shipped** — Astro outputs HTML, JS only where needed
- **Single page** — one index.astro with six scroll-snapped sections, no routing
- **Performance** — Lighthouse 90+ all categories
- **Responsive** — must work great on phones, tablets, desktops
