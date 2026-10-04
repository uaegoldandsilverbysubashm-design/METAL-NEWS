# Metals & Commodities Intelligence

A single-page, zero-build news dashboard for gold, silver, platinum, palladium, copper, mining, oil and macro headlines, with a built-in economic calendar. It pulls free public RSS feeds directly in the browser — no API keys, no backend.

## Economic Calendar tab

Besides the live MetaTrader/Tradays calendar widget, the tab includes a metals release tracker (times in Dubai/GST):

- countdown to the next release and a trading-session bar (Asia / London / New York)
- per-release outcome picker showing the usual reaction for gold, silver, platinum and copper
- forecast / actual entry with automatic surprise calculation
- a reaction journal (saved in the browser) and a reference table for the big monthly releases

### Live release feed

The release list updates itself. `api/calendar.js` is a Vercel serverless function (`/api/calendar`) that pulls the free weekly calendar feed from `nfs.faireconomy.media` (this week and next week), keeps the releases that matter for metals (US data, FOMC, ECB rates, China PMI), attaches the usual gold / silver / platinum / copper reaction rules, and returns JSON. Responses are cached at the edge for about 15 minutes, and the page refreshes every 30 minutes.

- The feed is unofficial and rate-limited. If it fails, the page falls back to the short built-in `EVENTS` list in `index.html` and says so in the status line.
- It carries consensus forecasts (pre-filled in the Forecast box) but not actual results. Type the actual in yourself.
- Releases with no reaction rule yet show no metal arrows. Add a rule to `RULES` in `api/calendar.js` to cover them.
- Confirm exact dates and times against official BLS, Federal Reserve and BEA calendars.
- To test the function locally use `npx vercel dev`. Opening `index.html` directly shows the built-in list only.

## Project structure

```
.
├── index.html     # the entire app (HTML + CSS + JS)
├── api/calendar.js # serverless function: live economic-calendar feed
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
