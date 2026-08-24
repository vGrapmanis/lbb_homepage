# Deployment

## Hosting — Cloudflare Pages

The site is hosted on **Cloudflare Pages**, connected directly to this GitHub repo. Every push to `main` triggers an automatic build and deploy — no manual upload step.

### Pages project settings

- **Root directory:** `latvian-blues-band`
- **Build command:** `npm run build`
- **Build output directory:** `dist`

### Updating content

To update show dates, member info, discography, or posters: edit the relevant JSON in `src/data/` (or add/remove files in `public/images/posters/`), then `git push` to `main`. Cloudflare Pages picks up the change and rebuilds/deploys automatically — usually live within a couple of minutes.

### Headers & redirects

Cloudflare Pages reads two special files from `public/` (copied into `dist/` at build time):

- `public/_headers` — security headers (`X-Frame-Options`, `Referrer-Policy`, HSTS, etc.)
- `public/_redirects` — 301 redirect from `www.latvianbluesband.lv` to the root domain

### DNS / domain

`latvianbluesband.lv` DNS is managed in Cloudflare. The domain is attached to the Pages project via **Pages project → Custom domains**, which Cloudflare wires up automatically since the zone is already on Cloudflare — no nameserver changes needed.

### Post-deploy checklist

- [ ] `https://latvianbluesband.lv` shows a valid HTTPS padlock
- [ ] `www` redirects to the root domain
- [ ] All sections, animations, embeds, newsletter, mailto work
- [ ] Sitemap reachable: `https://latvianbluesband.lv/sitemap.xml`
