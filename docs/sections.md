# Section Specifications

All six sections plus the navigation bar. Each section is a full-viewport scroll-snap section inside `index.astro`.

---

## Navigation Bar (always visible)

- **Fixed** at top of viewport, `position: fixed`, full width, `z-index: 50`
- **Left:** Band logo (clickable → `#home`)
- **Center:** Home | Media | Discography | Tour | About | Contact — anchor links
- **Right:** Social icons — Facebook, Instagram, Spotify, Deezer, YouTube (open in new tabs)
- **Active state:** Current section's nav link highlighted with accent color (Intersection Observer)
- **Background:** Transparent over hero, transitions to solid `--color-bg-elevated` when scrolled past first section
- **Mobile:** Hamburger menu with slide-in drawer, anchor links close drawer after click

---

## Section: HOME (`#home`)

The landing screen — big visual impact, band identity, call to action.

1. **Hero Banner** — Full-viewport band photo with dark gradient overlay + band name + tagline
2. **CTA Buttons:**
   - "See us live" → scrolls to `#tour` (fill-sweep animation)
   - "Get in touch" → scrolls to `#contact` (fill-sweep animation)

Components: `HeroBanner.astro`

---

## Section: MEDIA (`#media`)

Embedded music and video — let visitors hear and see the band.

1. **Section Title** — "Listen & Watch" with decorative accent
2. **Spotify Player** — Embedded Spotify artist player (dark theme, `theme=0`)
3. **YouTube Feature** — Latest music video or live performance (responsive 16:9 embed)
4. Scroll-triggered fade-in animations on both embeds

Components: `SpotifyPlayer.astro`, `YouTubeEmbed.astro`

### Spotify Embed

```html
<iframe
  style="border-radius:12px"
  src="https://open.spotify.com/embed/artist/ARTIST_ID?utm_source=generator&theme=0"
  width="100%"
  height="352"
  frameborder="0"
  allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture"
  loading="lazy"
>
</iframe>
```

### YouTube Embed

```html
<iframe
  width="100%"
  height="400"
  src="https://www.youtube.com/embed/VIDEO_ID"
  frameborder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowfullscreen
  loading="lazy"
>
</iframe>
```

- Wrap in responsive container with `aspect-ratio: 16/9`

---

## Section: DISCOGRAPHY (`#discography`)

Showcase the band's albums with links to streaming platforms.

1. **Section Title** — "Discography" with decorative accent
2. **Album Cards Grid** — Each card shows: cover art, title, year, streaming platform links (Spotify, Deezer, YouTube)
3. Cards have hover effect: subtle tilt (CSS perspective) + shadow deepen
4. Scroll-triggered staggered fade-in animation
5. Data source: `src/data/discography.json`

Components: `DiscographyCard.astro`

---

## Section: TOUR (`#tour`)

Upcoming and past live shows.

1. **Section Title** — "Upcoming Shows" with decorative accent
2. **Upcoming Shows** — ShowCards filtered by future dates, sorted chronologically
3. **Past Shows** — Collapsible/secondary list of past gigs (dimmed styling)
4. Each ShowCard: formatted date, venue name, city, optional ticket link button
5. ShowCard hover: left-border accent grow + slight translate-x
6. Data source: `src/data/shows.json`
7. If no upcoming shows: display "Stay tuned for upcoming shows" message

Components: `ShowCard.astro`

---

## Section: ABOUT (`#about`)

Band story and member profiles.

1. **Band Story** — 2-3 paragraphs about the band's history, style, mission
2. **Member Cards Grid** — Photo, name, instrument, short bio for each member
3. Cards with `socialLinks` show a dark overlay on hover (desktop) / tap (mobile) with social icon links
   - Overlay fades in at `opacity: 0 → 1` over 0.25 s
   - Each link shows an inline SVG icon + label text; opens in new tab
   - Supported platforms: `facebook`, `youtube`, `fiverr` (Fiverr uses branded green circle icon)
   - Multiple links per member supported (e.g. 3 Facebook entries with distinct labels)
   - Mobile: first tap shows overlay, second tap (outside a link) dismisses it
4. Responsive grid: 1 column (mobile) → 2 columns (tablet) → 3-4 columns (desktop)
5. Scroll-triggered staggered reveal animation
6. Data source: `src/data/members.json` — see `docs/data-schemas.md` for `socialLinks` schema

Components: `MemberCard.astro`

---

## Section: CONTACT (`#contact`)

How to reach the band — booking, newsletter, socials.

1. **Booking Email** — Large, prominent `mailto:` link styled as a button
   - `<a href="mailto:booking@latvianbluesband.lv?subject=Booking%20Inquiry">booking@latvianbluesband.lv</a>`
   - When clicked, opens user's default email client with pre-filled "To:" address
2. **Newsletter Signup** — Email input + "Join" button → submits to Mailchimp (see `docs/integrations.md`)
3. **Social Links** — Full social icons row with labels and hover effects
4. **Footer** — Copyright notice, sits at bottom of this section

Components: `NewsletterSignup.astro`, `Footer.astro`

**NO CONTACT FORM.** The email link opens the user's own email provider. Simple, no backend needed.
