# CLD — Clinical Labs Dashboard

Your entire lab history, finally readable. CLD turns years of scattered PDFs, portals, and paper results into one longitudinal record, with AI-assisted summaries, trend charts, a built-in Symptom Checker, and a printable physician report. Everything is stored in the browser; AI features call Groq directly with the user's own API key.

Built for physician review, not self-diagnosis.

## Layout

```
/                      landing page (index.html, self-contained: icons embedded, no external files or fonts)
/app/index.html        the app (single-file PWA)
/app/manifest.webmanifest
/app/sw.js             service worker, scope /app/
/app/*.png             PWA icons (icon-192/512, icon-maskable-512, apple-touch-icon, favicon-32/48, icon-1024 source)
/.nojekyll
```

## Deploy (GitHub Pages)

Settings → Pages → Deploy from branch → `main` / root. The landing page is the site root and the app lives at `/app/`.
All paths are relative, so it works under a project subpath or a custom domain.

## Icons

The landing page embeds its own icons, so it needs no image files.
The app needs real PNG files in `/app/` (browsers won't install from embedded icons):
`icon-192.png`, `icon-512.png`, `icon-maskable-512.png`, `apple-touch-icon.png`. `favicon-32/48.png` are optional, and `icon-1024.png` is only a source image.

## Releasing an update

Edit `app/index.html`, then bump `VERSION` in `app/sw.js` so installed copies drop their old cache.
The service worker is network-first, so a new deploy shows up on the next online load.

## Notes

- The service worker only handles same-origin GET requests; calls to `api.groq.com` are never cached.
- Groq key and model choices are stored in `localStorage` (`cld_groq_v1`, `cld_groq_models_v1`); lab data in `cld_data_v1`.
- Data is per-browser. Use Settings → Backup & Restore before clearing browser data or changing devices.
