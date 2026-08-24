# Deployment

## Hosting — Cloudflare Workers (static assets)

The site is hosted on **Cloudflare Workers**, using its static-assets deploy mode, connected directly to this GitHub repo via Cloudflare's Git integration (Workers Builds). Every push to `main` triggers an automatic build and deploy — no manual upload step.

### Build settings (Workers & Pages → lbb-homepage → Settings → Build)

- **Root directory:** `latvian-blues-band`
- **Build command:** `npm run build`
- **Deploy command:** `npx wrangler deploy`
- **Production branch:** `main`

`latvian-blues-band/wrangler.jsonc` tells Wrangler that `./dist` (the `astro build` output) is the static assets directory to publish.

### Updating content

To update show dates, member info, discography, or posters: edit the relevant JSON in `src/data/` (or add/remove files in `public/images/posters/`), then `git push` to `main`. Cloudflare picks up the change and rebuilds/deploys automatically — usually live within a couple of minutes.

### Headers

`latvian-blues-band/public/_headers` is copied into `dist/` at build time and applies security headers (`X-Frame-Options`, `Referrer-Policy`, HSTS, etc.) to every response.

Note: Cloudflare's static-assets `_redirects` file only supports relative-path redirects, not domain-to-domain ones — so the `www` → root redirect is *not* done via a file in this repo. It's configured as a Cloudflare **Redirect Rule** at the zone level (Rules → Redirect Rules) in the dashboard instead.

### DNS / domain

`latvianbluesband.lv` DNS is managed in Cloudflare. The domain is attached to the Worker via **Workers & Pages → lbb-homepage → Settings → Domains & Routes**, which wires up DNS automatically since the zone is already on Cloudflare — no nameserver changes needed.

### Post-deploy checklist

- [ ] `https://latvianbluesband.lv` shows a valid HTTPS padlock
- [ ] `www` redirects to the root domain
- [ ] All sections, animations, embeds, newsletter, mailto work
- [ ] Sitemap reachable: `https://latvianbluesband.lv/sitemap.xml`
