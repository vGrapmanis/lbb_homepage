# Deployment & Domain Migration

Launching latvianbluesband.lv on Netlify.

---

## Before Launch Day

1. **Deploy to Netlify** — push code to GitHub, connect repo in Netlify dashboard
2. Netlify auto-assigns a temporary URL like `latvian-blues-band.netlify.app`
3. **Test everything** on this temporary URL — all sections, animations, embeds, newsletter, mailto, mobile

---

## Launch Day

### Step 1: Netlify Dashboard

- Go to Site settings → Domain management → Add custom domain
- Enter: `latvianbluesband.lv`
- Also add: `www.latvianbluesband.lv` (redirect www → root)

### Step 2: DNS Settings (at your domain registrar)

**Option A: Netlify DNS (Recommended — simplest)**

- Change nameservers at registrar to Netlify's:
  - `dns1.p05.nsone.net`
  - `dns2.p05.nsone.net`
  - `dns3.p05.nsone.net`
  - `dns4.p05.nsone.net`
- Netlify gets full DNS control, auto-provisions HTTPS/SSL

**Option B: External DNS (keep registrar's DNS)**

- Add A record: `@` → `75.2.60.5` (Netlify load balancer)
- Add CNAME record: `www` → `latvian-blues-band.netlify.app`
- Remove old A/CNAME records pointing to previous hosting
- In Netlify: Domain settings → Verify DNS → provision SSL

### Step 3: DNS Propagation

- Takes 15 minutes to 48 hours (usually under 2 hours)
- Check at: whatsmydns.net

### Step 4: Post-Propagation Verification

- Verify HTTPS: `https://latvianbluesband.lv` shows green lock
- Verify `www` redirects to root domain
- Submit sitemap to Google Search Console: `https://latvianbluesband.lv/sitemap.xml`
- Update GA4 property URL if needed

---

## Discarding the Old Website

- **Shared hosting:** Cancel hosting plan after DNS confirmed (give 48h)
- **VPS:** Shut down server after confirming everything works
- **Other platform (WordPress, Wix, etc.):** Delete/cancel the account
- **Keep a backup** of old site content before deleting anything

---

## Ongoing: Auto-Deploys

After launch, every push to `main` on GitHub automatically deploys to Netlify.
To update show dates, member info, or discography — edit JSON files in `src/data/` and push.

---

## netlify.toml

```toml
[build]
  command = "npm run build"
  publish = "dist"
```
