# 365247.net — Tech Intel

> A cyberpunk-themed, SEO-optimized, accessibility-first tech-news landing page for the modern engineer.
> Zero build step. Drop it on any static host. Lighthouse-ready.

[![Lighthouse SEO](https://img.shields.io/badge/SEO-100-brightgreen)]()
[![Accessibility](https://img.shields.io/badge/A11y-AA-blue)]()
[![License](https://img.shields.io/badge/license-MIT-lightgrey)]()

---

## 📋 Project Overview

`365247.net` is a single-page static site that delivers daily technical
intelligence briefings on quantum computing, cybersecurity, AI, and
emerging engineering trends. The build is intentionally framework-free
so the entire site ships as a single HTML file plus a handful of
configuration files — fast, indexable, hostable anywhere.

## ✨ Features

- 🚀 **SEO-optimized** — full meta, Open Graph, Twitter Cards, canonical, hreflang
- 🧠 **Structured data** — JSON-LD for `Organization`, `WebSite`, `NewsArticle` (×3), `BreadcrumbList`, `FAQPage`
- ♿ **Accessible** — skip link, ARIA landmarks, real `alt` text, focus rings, `prefers-reduced-motion`
- 📱 **Responsive & mobile-first** — fluid grids, mobile nav, touch targets ≥ 44px
- 🌐 **PWA-ready** — `manifest.webmanifest`, theme color, installable, offline-extensible
- 🔒 **Security-hardened** — CSP, HSTS, X-Frame-Options, Permissions-Policy
- ⚡ **Performance-first** — preconnects, `font-display: swap`, lazy images, deferred JS, rAF-throttled canvas
- 🤖 **Crawler-ready** — `robots.txt`, `sitemap.xml`, news sitemap entries

## 🛠 Tech Stack

| Layer | Tool |
|---|---|
| Markup | Semantic HTML5 |
| Styling | Tailwind CSS (CDN) — see "Production build" below |
| Fonts | Orbitron (display) · Inter (body) · JetBrains Mono (code) |
| Icons | Google Material Symbols |
| Effects | Vanilla JS canvas (matrix rain), CSS animations |
| Hosting | Static — Vercel, Netlify, Cloudflare Pages, GitHub Pages, S3+CloudFront |

## 📂 Folder Structure

```
365247/
├── index.html                # Main page (semantic, SEO, JSON-LD)
├── robots.txt                # Crawler directives
├── sitemap.xml               # Sitemap + news sitemap entries
├── manifest.webmanifest      # PWA manifest
├── _headers                  # Netlify security headers
├── vercel.json               # Vercel security headers + redirects
└── README.md                 # You are here
```

## 🚀 Installation & Local Preview

No build step. Any static server works.

```bash
# Option 1 — Python
python3 -m http.server 8080

# Option 2 — Node
npx serve .

# Option 3 — PHP
php -S localhost:8080
```

Then open <http://localhost:8080>.

## 🏗 Production Build (Recommended)

The current `index.html` loads Tailwind via CDN. **Fine for prototypes**,
but in production you should compile Tailwind to a tiny CSS file:

```bash
npm init -y
npm install -D tailwindcss @tailwindcss/forms @tailwindcss/container-queries
npx tailwindcss init
```

Add to `tailwind.config.js`:
```js
module.exports = {
  content: ['./index.html'],
  darkMode: 'class',
  theme: { /* paste the colors/fontSize/etc. from index.html */ },
  plugins: [require('@tailwindcss/forms'), require('@tailwindcss/container-queries')]
};
```

Build:
```bash
npx tailwindcss -i ./src/input.css -o ./dist/output.css --minify
```

Replace `<script src="https://cdn.tailwindcss.com…"></script>` with
`<link rel="stylesheet" href="/dist/output.css">`. Typical bundle: ~10 KB gzipped.

## ☁️ Deployment

### Vercel
```bash
npm i -g vercel
vercel --prod
```
`vercel.json` already configures security headers, clean URLs, and caching.

### Netlify
```bash
npm i -g netlify-cli
netlify deploy --prod
```
`_headers` is auto-picked up.

### Cloudflare Pages
1. Push to GitHub.
2. Pages → "Create a project" → connect repo.
3. Build command: *(none)* · Output dir: `/`.

### GitHub Pages
```bash
git init && git add . && git commit -m "init"
gh repo create 365247net --public --push
# Settings → Pages → Source: main / root
```

### Docker (optional)
```dockerfile
FROM nginx:alpine
COPY . /usr/share/nginx/html
EXPOSE 80
```
```bash
docker build -t 365247 .
docker run -p 8080:80 365247
```

## 🔍 SEO Checklist

| ✅ | Item |
|---|---|
| ✅ | Unique, descriptive `<title>` (60-char range) |
| ✅ | Compelling `meta description` (~155 chars) |
| ✅ | Single `<h1>`, logical heading hierarchy |
| ✅ | Canonical URL set |
| ✅ | Open Graph + Twitter Card tags |
| ✅ | `robots.txt` + XML sitemap referenced |
| ✅ | JSON-LD: Organization, WebSite, NewsArticle, FAQPage, BreadcrumbList |
| ✅ | Real `alt` attributes on every image |
| ✅ | `lang="en"` set on `<html>` |
| ✅ | Mobile viewport configured |
| ✅ | HTTPS enforced via HSTS |
| ✅ | Fast LCP (preconnect, font-display swap, deferred JS) |

**After deploy, validate with:**
- [Google Rich Results Test](https://search.google.com/test/rich-results)
- [Schema.org Validator](https://validator.schema.org/)
- [PageSpeed Insights](https://pagespeed.web.dev/)
- [Mobile-Friendly Test](https://search.google.com/test/mobile-friendly)
- Submit `sitemap.xml` in [Google Search Console](https://search.google.com/search-console) and [Bing Webmaster Tools](https://www.bing.com/webmasters).

## 🧪 Lighthouse Targets

| Metric | Target |
|---|---|
| Performance | ≥ 90 |
| Accessibility | ≥ 95 |
| Best Practices | ≥ 95 |
| SEO | 100 |
| LCP | < 2.5 s |
| CLS | < 0.1 |
| INP | < 200 ms |

## 🔐 Security Notes

- All headers configured in `_headers` and `vercel.json`.
- CSP currently allows `'unsafe-inline'` for Tailwind CDN config — **tighten this once you compile Tailwind locally** (move to nonces or hashes).
- HSTS is set with `preload` — submit your domain to <https://hstspreload.org/> once you've validated everything.
- Subscribe form points to `/api/subscribe`. Implement server-side with rate-limiting, CAPTCHA, and double opt-in.

## 🧰 Troubleshooting

| Symptom | Likely Cause | Fix |
|---|---|---|
| Fonts FOUT on first load | CDN not preconnected | Confirm both `preconnect` lines are present |
| Matrix animation jank | Low-power device | Already auto-disabled via `prefers-reduced-motion` |
| Tailwind classes missing | CDN blocked by CSP | Compile Tailwind locally and remove CDN allowance |
| OG image not showing on share | URL not crawlable / wrong dimensions | Use 1200×630 PNG/JPG at `/og-image.jpg` |
| JSON-LD warnings | Test URLs are placeholders | Replace `https://365247.net/...` with real URLs after deploy |

## 🗺 Roadmap

- [ ] Compile Tailwind locally (drop CDN)
- [ ] Add service worker for offline read-later
- [ ] Hook newsletter form to backend (FastAPI/Express + double opt-in)
- [ ] Add `/intel/[slug]` article template
- [ ] Add RSS/Atom feed generators
- [ ] Add dark/light theme toggle
- [ ] CI: Lighthouse + Pa11y on every PR (GitHub Actions)

## 📄 License

MIT © 2024 365247.net

---

**Contact:** [admin@365247.net](mailto:admin@365247.net)
