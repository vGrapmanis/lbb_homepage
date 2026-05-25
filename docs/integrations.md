# Integrations

Mailchimp newsletter, Google Analytics 4, and SEO configuration.

---

## Mailchimp Newsletter

### How It Works

1. User enters email in the newsletter input field (Contact section)
2. User clicks "Join" button
3. Email submitted to Mailchimp via their embedded form action URL
4. Mailchimp stores subscriber with metadata (signup source, date, IP)
5. Owner uses Mailchimp audience features for remarketing emails

### Implementation: Embedded Form

```html
<!-- NewsletterSignup.astro -->
<form
  action="https://latvianbluesband.us{X}.list-manage.com/subscribe/post?u={USER_ID}&id={LIST_ID}"
  method="POST"
  target="_blank"
  class="newsletter-form"
>
  <!-- Honeypot anti-spam (hidden from users) -->
  <div style="position: absolute; left: -5000px;" aria-hidden="true">
    <input type="text" name="b_{USER_ID}_{LIST_ID}" tabindex="-1" value="" />
  </div>

  <input
    type="email"
    name="EMAIL"
    placeholder="Your email address"
    required
    class="newsletter-input"
  />
  <button type="submit" class="btn newsletter-btn">Join</button>
</form>
```

### Mailchimp Setup Steps (Owner must do)

1. Create free Mailchimp account at mailchimp.com (free up to 500 subscribers)
2. Create an "Audience" (mailing list)
3. Go to Audience → Signup forms → Embedded forms
4. Copy the form action URL — contains `USER_ID` and `LIST_ID`
5. Replace placeholder values in `NewsletterSignup.astro`

### Subscriber Metadata Available

- Email address, signup date/time, signup source
- IP address / geolocation
- Engagement metrics (opens, clicks) — builds over time
- Custom tags for segmentation and remarketing

---

## Google Analytics 4 (GA4)

### Implementation

Add in `BaseLayout.astro` inside `<head>`:

```html
<script
  async
  src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"
></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag() {
    dataLayer.push(arguments);
  }
  gtag("js", new Date());
  gtag("config", "G-XXXXXXXXXX");
</script>
```

### Setup Steps (Owner must do)

1. Go to analytics.google.com
2. Create a new GA4 property for "latvianbluesband.lv"
3. Get the Measurement ID (starts with `G-`)
4. Replace `G-XXXXXXXXXX` in the script

### Automatic Tracking

- Page views, user demographics/location, device types, traffic sources, session duration

### Custom Events to Track

```javascript
// Section views (in scroll observer)
gtag("event", "section_view", { section_name: "tour" });

// Newsletter signup click
gtag("event", "newsletter_signup", { method: "mailchimp" });

// Email contact click
gtag("event", "contact_click", { method: "mailto" });

// Social link clicks
gtag("event", "social_click", { platform: "spotify" });

// Ticket link clicks
gtag("event", "ticket_click", { show: "venue-date" });
```

---

## SEO Implementation

### Meta Tags (in `BaseLayout.astro` `<head>`)

```html
<title>Latvian Blues Band — Blues Music from Latvia</title>
<meta
  name="description"
  content="Official website of Latvian Blues Band. Live shows, music, and booking information."
/>
<meta name="viewport" content="width=device-width, initial-scale=1" />
<meta charset="UTF-8" />
<link rel="canonical" href="https://latvianbluesband.lv" />
<meta name="robots" content="index, follow" />
<meta name="language" content="en" />

<!-- Open Graph / Facebook -->
<meta property="og:type" content="website" />
<meta property="og:url" content="https://latvianbluesband.lv" />
<meta
  property="og:title"
  content="Latvian Blues Band — Blues Music from Latvia"
/>
<meta
  property="og:description"
  content="Official website of Latvian Blues Band. Live shows, music, and booking."
/>
<meta
  property="og:image"
  content="https://latvianbluesband.lv/images/band/og-image.jpg"
/>

<!-- Twitter Card -->
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:title" content="Latvian Blues Band" />
<meta
  name="twitter:description"
  content="Blues music from Latvia. Live shows, music, and booking."
/>
<meta
  name="twitter:image"
  content="https://latvianbluesband.lv/images/band/og-image.jpg"
/>
```

### Structured Data (JSON-LD in `BaseLayout.astro`)

```html
<script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "MusicGroup",
    "name": "Latvian Blues Band",
    "url": "https://latvianbluesband.lv",
    "genre": "Blues",
    "foundingLocation": { "@type": "Place", "name": "Latvia" },
    "sameAs": [
      "https://facebook.com/latvianbluesband",
      "https://instagram.com/latvianbluesband",
      "https://open.spotify.com/artist/ARTIST_ID",
      "https://www.deezer.com/artist/ARTIST_ID",
      "https://www.youtube.com/@latvianbluesband"
    ]
  }
</script>
```

### robots.txt (in `public/`)

```
User-agent: *
Allow: /
Sitemap: https://latvianbluesband.lv/sitemap.xml
```

### Sitemap

- Use Astro's `@astrojs/sitemap` integration
- Set `site: 'https://latvianbluesband.lv'` in `astro.config.mjs`

### SEO Checklist

- [ ] Every image has descriptive `alt` text
- [ ] Page title under 60 characters
- [ ] Meta description under 160 characters
- [ ] OG image is 1200x630px
- [ ] Structured data validates at search.google.com/structured-data/testing-tool
- [ ] Sitemap generated and submitted to Google Search Console
- [ ] robots.txt accessible
- [ ] All text is real HTML (not baked into images)
- [ ] Lighthouse performance 90+
- [ ] Mobile-friendly
