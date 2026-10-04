# Metals & Commodities Intelligence

A single-page, zero-build news dashboard for gold, silver, platinum, palladium, copper, mining, oil and macro headlines, with a built-in economic calendar. It pulls free public RSS feeds directly in the browser — no API keys, no backend.

## Project structure

```
.
├── index.html     # the entire app (HTML + CSS + JS)
├── vercel.json    # Vercel config (clean URLs, security headers)
├── .gitignore
└── README.md
```

## Run locally

Open `index.html` in a browser, or serve the folder:

```bash
npx serve .
# or
python3 -m http.server 3000
```

## Deploy to Vercel

### Option 1 — Git integration (recommended)

1. Create a new empty repository on GitHub, GitLab or Bitbucket.
2. From this folder:
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin <YOUR_REPO_URL>
   git push -u origin main
   ```
3. Go to [vercel.com/new](https://vercel.com/new) and import the repository.
4. Use these settings:
   - **Framework Preset:** Other
   - **Build Command:** *(leave empty)*
   - **Output Directory:** *(leave empty — the root is served)*
   - **Install Command:** *(leave empty)*
5. Click **Deploy**. Every push to `main` redeploys automatically.

### Option 2 — Vercel CLI

```bash
npm i -g vercel
vercel          # preview deployment
vercel --prod   # production deployment
```

## Notes

- **RSS sources:** each feed is fetched through `api.rss2json.com` first, with a CORS proxy (`corsproxy.io`, configurable in the in-app settings) as a fallback. Both are free third-party services with rate limits, so some feeds may occasionally be unavailable.
- **Economic calendar:** provided by the MetaTrader / Tradays widget and loaded the first time the Calendar tab is opened.
- **Settings and theme** (light/dark) are stored in the visitor's browser via `localStorage`.
- If you later need reliable, rate-limit-free feeds, move the RSS fetching into a Vercel Serverless Function (`/api/feeds`) and call it from the page.
