# Deployment

## Current Setup — FTP via FileZilla

The site is deployed manually by building locally and uploading the output via FTP.

### Deploy steps

1. `npm run build` — produces the static site in `dist/`
2. Open **FileZilla**, connect to the hosting server (ask the site owner for FTP credentials)
3. Upload the contents of `dist/` to the server's web root (e.g. `public_html/` or `www/`)
4. Overwrite existing files when prompted

> Every deploy is a full overwrite of the `dist/` directory. There is no incremental or diff-based upload.

### Updating content

To update show dates, member info, or discography: edit the relevant JSON in `src/data/`, rebuild, and re-upload via FTP.

---

## Future Migration — Netlify

A migration to **Netlify** (free tier) is planned. It would replace the manual FTP step with automatic deploys on every push to `main`.

### Why migrate

- Auto-deploy on `git push` — no manual upload
- Free SSL/HTTPS provisioned automatically
- Branch previews, rollbacks, deploy logs
- Built-in CDN

### Migration steps (when ready)

1. Push repo to GitHub (already there)
2. Connect the GitHub repo to a new Netlify site in the Netlify dashboard
3. Set build command: `npm run build`, publish directory: `dist`
4. Netlify assigns a temporary URL (e.g. `latvian-blues-band.netlify.app`) — test everything there

### DNS cutover (latvianbluesband.lv)

**Option A — Netlify DNS (simplest)**

- Change nameservers at the domain registrar to Netlify's:
  - `dns1.p05.nsone.net`
  - `dns2.p05.nsone.net`
  - `dns3.p05.nsone.net`
  - `dns4.p05.nsone.net`
- Netlify provisions HTTPS automatically

**Option B — External DNS (keep registrar's DNS)**

- Add A record: `@` → `75.2.60.5` (Netlify load balancer)
- Add CNAME: `www` → `latvian-blues-band.netlify.app`
- Remove old A/CNAME records pointing to the FTP host
- In Netlify: Domain settings → Verify DNS → provision SSL

DNS propagation takes 15 min – 48 h. Verify at whatsmydns.net.

### Post-cutover checklist

- [ ] `https://latvianbluesband.lv` shows green lock
- [ ] `www` redirects to root domain
- [ ] All sections, animations, embeds, newsletter, mailto work
- [ ] Submit sitemap to Google Search Console: `https://latvianbluesband.lv/sitemap.xml`
- [ ] Update GA4 property URL if needed
- [ ] Cancel old shared hosting after confirming everything works (wait 48 h after DNS cutover)

### netlify.toml

```toml
[build]
  command = "npm run build"
  publish = "dist"
```
