# FutureAutoLab – deploy & maintenance

## Deploy (Netlify)
1. Push this folder to GitHub (or drag-drop the folder into Netlify).
2. Netlify reads `netlify.toml` -> publishes the site root and deploys `netlify/functions/*` automatically.
3. Forms: Site -> Forms -> newsletter -> Notifications -> add your email (fallback storage for signups).

## Backend (Netlify Functions)
| Endpoint | What it does |
|---|---|
| `/.netlify/functions/news?topic=ev/reviews/tech/videos` | Fetches Google News / YouTube RSS server-side, 15-min cache, serves stale if upstream fails |
| `/.netlify/functions/subscribe` | Validates email, honeypot + rate-limit, then Buttondown (if key set) or Netlify Forms. Reports success only if upstream accepted |
| `/.netlify/functions/contact` | Validates, honeypot + rate-limit, forwards to Formspree |

Optional environment variables (Site settings -> Environment variables):
- `BUTTONDOWN_API_KEY` - real double-opt-in newsletter (recommended)
- `FORMSPREE_ENDPOINT` - change contact destination (default: current form id)
- `SITE_URL` - your custom domain once you have one (e.g. https://futureautolabtech.com)

## Tailwind CSS (no CDN)
Compiled files: `css/tw-site.css`, `css/tw-tools.css`. After adding new Tailwind classes run
`npm install && npm run build:css`. Bump `?v=3` in the HTML when you change css/js.

## Before applying to AdSense - checklist
- [ ] Custom domain (.com) connected in Netlify, then run `python3 set-domain.py https://yourdomain.com` (see below) and push
- [ ] AdSense -> Privacy & messaging -> create the EU/UK GDPR message (Google-certified CMP), then set USE_GOOGLE_CMP = true in js/consent.js
- [ ] Verify every claim in guides against official sources (IRS.gov, afdc.energy.gov)
- [x] Single author (Bilal Ahmad Alvi, CEO) on every article, JSON-LD and the About page
- [ ] Use ONE contact email everywhere (about / contact / privacy)
- [ ] Submit sitemap in Google Search Console; keep publishing original articles (aim 20+ quality pages)
- [ ] Ads are kept only on article/tool pages. To add ads elsewhere, add the AdSense script to that page's head.

## Secrets (important)
- `netlify/functions/_keys.js` is NOT in this package any more. Never commit API keys to GitHub.

## Switching to your own domain
> **Already applied:** this version is rebranded to FutureAutoLab and uses https://futureautolabtech.com in every canonical, sitemap, robots.txt, JSON-LD and og tag, so `set-domain.py` is no longer needed. What is left on your side: add the domain in Netlify and make it primary, set `SITE_URL` to https://futureautolabtech.com, verify the new property in Search Console, submit the sitemap.

1. Buy the domain, add it in Netlify -> Domain management, wait for HTTPS.
2. From the folder that contains `set-domain.py` run: `python3 set-domain.py https://yourdomain.com`
   (replaces the netlify.app address in canonicals, sitemap, robots.txt, JSON-LD and og tags).
3. Set `SITE_URL` in Netlify environment variables, push to GitHub, submit the new sitemap in Search Console, then apply for AdSense.

## Notes on this version
- The Video & Image Finder (videos page and media function) has been removed.
- /regenerative-braking-explained-2026 was a near-duplicate of the one-pedal driving guide; it now 301-redirects to /regenerative-braking-one-pedal-driving-2026.html.
- New hub page: /guides.html lists every guide.
