# Metals & Commodities Intelligence

A zero-build dashboard for gold, silver, platinum, palladium, copper, mining, oil and macro news, with a built-in economic calendar and a metals release tracker. Plain HTML, CSS and JavaScript, plus one small serverless function. Deploys to Vercel straight from GitHub.

## Features

### News Feed
- Aggregates free public RSS feeds (Kitco, Mining.com, FXStreet, Federal Reserve, ECB and more) in the browser
- Fetches via `rss2json`, with a CORS proxy as fallback, and a timeout on every request
- Removes near-duplicate headlines across sources
- Tags each story by metal or topic and scores sentiment
- Filter tabs: All, Gold, Silver, Platinum, Palladium, Copper, Mining, Oil & Energy, Macro
- Light and dark theme, saved in the browser

### Economic Calendar
- Live MetaTrader / Tradays calendar widget, loaded the first time the tab is opened
- **Metals release tracker** (times in Dubai / GST):
  - countdown to the next release and a trading-session bar (Asia / London / New York)
  - pick an outcome to see the usual reaction for gold, silver, platinum and copper
  - forecast and actual entry with automatic surprise calculation
  - reaction journal saved in the browser, plus a reference table of the big monthly releases
- **Live release list:** `/api/calendar` pulls the free weekly calendar feed, keeps the releases that matter for metals, attaches the reaction rules and returns clean JSON. Consensus forecasts are pre-filled.

## How the live calendar works

```
Browser  ->  /api/calendar (Vercel function, cached ~15 min)  ->  weekly calendar feed
```

- The page refreshes the list every 30 minutes.
- If the feed is unreachable, the page falls back to a short built-in list and says so in the status line.
- The feed is **unofficial and rate-limited**, and it has **no actual results**. Type the actual in yourself.
- Releases with no reaction rule show no metal arrows. Add a rule to `RULES` in `api/calendar.js` to cover them.
- Always confirm exact dates and times against the official BLS, Federal Reserve and BEA calendars.

## Limitations

- RSS goes through free third-party services (`rss2json`, `corsproxy.io`) with rate limits, so a feed may occasionally be unavailable.
- Headline sentiment and metal tags are keyword based, so treat them as a guide.
- This is an information tool, not trading or financial advice.
