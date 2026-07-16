# Nexus Dashboard

A self-hosted personal dashboard and bookmark manager. Lives in your GitHub repo, works in any browser, on any device — no accounts, no sync services, no third-party dependencies for your data.

![Theme](https://img.shields.io/badge/theme-dark%20%2F%20light-c8a96e) ![Storage](https://img.shields.io/badge/storage-GitHub%20JSON-7eb8c9) ![Dependencies](https://img.shields.io/badge/dependencies-none-6ec98a) ![Mobile](https://img.shields.io/badge/layout-responsive-c96eb4)

---

## Layout

Nexus uses a persistent header, a glance strip, and a three-tab interface.

### Header
Logo, dual clock (local + UTC), and a unified search omnibar with a Links/Web mode toggle. Always visible regardless of which tab is active.

### Glance Strip
A slim persistent bar between the header and tab nav. Always visible. No interaction required.
- **Weather** — current conditions, temperature in °F and °C, high/low, feels like. Set your location once in Settings.
- **Markets** — DOW, NASDAQ, S&P 500 end-of-day prices with change and percentage. Market-hours aware.

### Tab 1 — Bookmarks
Full-width bookmark manager. Categories, links, search — everything you'd expect.

### Tab 2 — Workspace
A 2×2 panel grid (collapses to single column on mobile):
- **Web Search** — DuckDuckGo, Perplexity, or Brave; engine remembered between sessions
- **Applets** — Writing Prompt, Fidget widget, Reflect, News Brief
- **Scratch Pad** — Quick notes, auto-saved locally, syncs to GitHub with the Save button
- **RSS Feeds** — Up to 10 feeds, merged chronological stream, per-feed filter

### Tab 3 — Settings
Two-column layout (collapses to single column on mobile):
- **Appearance** — Dark/light mode toggle, global accent colour picker
- **Data** — Weather location, import bookmarks, export bookmarks, GitHub sync status and token management

---

## Features

### Bookmarks
- **Collapsible categories** — colour-coded left border accents, favicons, instant search
- **Pinned category** — pin any one category as a permanent anchor at the top; always open, visually distinct with a ★ PINNED badge
- **Category colours** — inline colour picker when renaming; choose from 30 curated colours or reset to default
- **Inline editing** — rename categories and pick colours in one step; add, edit, and delete links without touching code
- **Move links** — reassign any link to a different category via the edit modal
- **Global quick-add** — add a link without expanding any category first
- **Alphabetical sort** — sort all categories and their links A–Z in one click
- **Drag-and-drop reordering** — drag categories into any order; drag links within a category to reorder them
- **GitHub-backed storage** — bookmarks saved to `data.json` via the GitHub API with live sync status; auto-retries on SHA conflicts

### Glance Strip — Weather
- Powered by [Open-Meteo](https://open-meteo.com) — free, no API key required
- Displays current condition with emoji icon, temperature in both °F and °C, today's high/low, and feels-like temperature
- Location set by city name search (Open-Meteo geocoding) or browser geolocation — stored in `localStorage`, never in the repo
- Falls back to cached data if the network is unavailable
- Location managed from **Settings → Data → Weather Location**

### Glance Strip — Markets
- DOW (`^DJI`), NASDAQ (`^IXIC`), S&P 500 (`^GSPC`) — end-of-day data only
- Fetches via Yahoo Finance (unofficial endpoint) through corsproxy.io; falls back to Alpha Vantage free tier if Yahoo is unavailable
- Market-hours aware: shows Open / Pre-market / After hours / Weekend status; skips refresh outside trading hours if cache is recent
- Data cached in `localStorage`; shown instantly on load while fresh data fetches in background

### Appearance
- **Dark / light theme** — warm dark and parchment light modes, toggled from Settings
- **Global accent colour** — five choices (Gold, Teal, Ember, Violet, Slate) applied dashboard-wide; persisted to `localStorage` under `nexus_accent`
- **Dual clock** — local time displayed in the accent colour; UTC date and time below it in muted text; both always visible in the header
- **Responsive layout** — works on desktop, tablet, and mobile

### Import & Export
- **Browser import** — import from Safari or Firefox HTML exports with full preview: rename categories, merge into existing ones, skip duplicates
- **Export** — download a clean `nexus-data.json` backup any time; both accessible from the Settings tab

### Web Search
- **Three engines** — DuckDuckGo, Perplexity, and Brave Search, switchable with one click
- **DDG optimised** — no AI answers, no ads, dark mode, and privacy settings baked into the URL
- **Brave** — `summary=0`, `country=us`, `lang=en-us` params included for cleaner results
- **Persistent preference** — last used engine remembered between sessions

### RSS Feeds
- **Up to 10 feeds** — add, remove, and manage feeds via an Edit panel that tucks away when not in use
- **Combined view** — all feeds merged into a single chronological stream with source labels
- **Per-feed filter** — dropdown to view any single feed in isolation
- **7-day filter** — items older than 7 days are automatically hidden
- **Dual proxy** — tries rss2json first, falls back to corsproxy.io with native XML parsing; supports both RSS and Atom formats
- **Refresh button** — reload all feeds on demand

### Scratch Pad
- Auto-saves to `localStorage` as you type
- **Save button** — syncs the current content to GitHub alongside your bookmarks in `data.json`

### Applets
Launched from the Workspace tab Applets panel:

- **Writing Prompt** — four tabs (All / Scenes / Scenarios / Sparks), weighted draw history, inline Write textarea, export as `.txt`
- **Fidget** — dial, ripple field, clicky button, rocker switch, spring-back slider, five pentatonic toggle switches; sound on/off; inherits the global accent colour
- **Reflect** — daily practice applet; select one or more elements (Water, Earth, Fire, Air) to capture the feel of the day; five structured yes/somewhat/no questions (Failures, Successes, Learning, Creating, Self-care); optional freeform note; export as dated `.md` file
- **News Brief** — AI-synthesised daily news brief via Claude API; fetches headlines from wire services; generates structured sections (World, US, Financial, Tech, Science, Health, AI); per-item Perplexity deep-dive links; on-demand refresh; cached between sessions; requires a Cloudflare Worker proxy (see setup below)

### Keyboard Shortcuts
- `1` `2` `3` — switch between Bookmarks, Workspace, and Settings tabs
- `/` — focus the search omnibar
- `\` — toggle search mode between Links and Web
- `Esc` — close any open modal, or clear and blur the search bar if focused

Shortcut hints are always visible in the footer.

### General
- **Single file** — everything in one `index.html`, no build tools, no frameworks, no dependencies
- **Custom favicon** — inline SVG node-and-spokes design in dashboard gold

---

## Setup

### 1. Fork or clone this repo
Or upload `index.html` and `data.json` to any existing GitHub repo.

### 2. Enable GitHub Pages
Go to your repo **Settings → Pages**, set Source to `main` branch, `/ (root)`, and save. Your dashboard will be live at:

```
https://yourusername.github.io/reponame
```

### 3. Create a Personal Access Token
1. Go to [github.com/settings/tokens](https://github.com/settings/tokens)
2. Choose **Tokens (classic)** → **Generate new token (classic)**
3. Give it a name, set an expiration (90 days recommended), and check the **`repo`** scope
4. Generate and copy the token — GitHub only shows it once

### 4. Enter your token in the dashboard
Open your dashboard URL. On first visit, a prompt will appear asking for your token. Paste it in and hit Save. It's stored only in your browser's `localStorage` — never in the repo.

That's it. Every bookmark change you make will now sync silently back to `data.json` in this repo.

---

## Weather Setup

No API key required. Open your dashboard, go to **Settings → Data → Weather Location**, type a city name and select from the results. Your coordinates are stored in `localStorage` only — never in the repo.

To use your device's GPS instead, click **Use my location instead** below the search field.

---

## Markets Setup

Markets data works out of the box via Yahoo Finance. If Yahoo is unavailable, it falls back to [Alpha Vantage](https://www.alphavantage.co/support/#api-key). The free Alpha Vantage key is included in the source — 25 requests/day is more than sufficient for three tickers checked occasionally.

If you fork this repo and want your own key, replace `AV_KEY` near the top of the `<script>` block in `index.html`.

---

## News Brief Setup

The News Brief applet requires a Cloudflare Worker to proxy requests to the Anthropic API. This keeps your API key off the client and handles CORS.

### 1. Create a Cloudflare Worker
1. Go to [dash.cloudflare.com](https://dash.cloudflare.com) → **Workers & Pages** → **Create** → **Create Worker**
2. Name it (e.g. `nexus-brief`)
3. Click **Edit code** and paste the contents of `nexus-brief-worker.js`
4. Click **Deploy**

### 2. Add your Anthropic API key
1. In the Worker settings, go to **Settings → Variables and Secrets**
2. Add a new variable: name `ANTHROPIC_API_KEY`, value is your key from [console.anthropic.com](https://console.anthropic.com)
3. Set it as a **Secret** (encrypted)
4. Save and deploy

### 3. Update the Worker URL in index.html
Find the line in `index.html`:
```js
const response = await fetch('https://nexus-brief.yourname.workers.dev', {
```
Replace the URL with your actual Worker URL.

### 4. Lock the Worker to your domain
Update the `ALLOWED_ORIGIN` constant at the top of `nexus-brief-worker.js` to match your GitHub Pages domain exactly before deploying.

The News Brief uses the Anthropic API on a pay-per-use basis. At roughly $0.01 per refresh, $5 in credits lasts a long time.

---

## Migrating from an earlier version

If you were using a version of Nexus that stored data in `bookmarks.json`, rename that file before deploying:

1. In your GitHub repo, open `bookmarks.json`
2. Click the pencil (edit) icon
3. Change the filename to `data.json`
4. Commit the change

Then deploy the new `index.html`. All existing bookmarks will load normally.

---

## Importing Bookmarks

Click **Import** in the Settings tab and drop your browser's bookmark export file:

- **Safari:** File → Export Bookmarks
- **Firefox:** Bookmarks → Manage Bookmarks → Import and Backup → Export Bookmarks to HTML

The import preview lets you rename categories, merge them into existing ones, and skip duplicates before committing anything. Nested subfolders are flattened automatically.

---

## Web Search

| Engine | Notes |
|--------|-------|
| **DuckDuckGo** | No AI · No ads · Dark mode · Private — all baked into the search URL |
| **Perplexity** | AI-powered answers · Account settings carry over when logged in |
| **Brave Search** | Independent index · AI summary suppressed · US/English results |

Your last-used engine is remembered between sessions. Use `\` to toggle between Links and Web search mode from the keyboard.

---

## Accent Colours

The global accent colour is set from **Settings → Appearance**. Five options:

| Name | Dark mode | Light mode |
|------|-----------|------------|
| Gold (default) | `#c8a96e` | `#9a7a3a` |
| Teal | `#7eb8c9` | `#3a7a9a` |
| Ember | `#c9784a` | `#a05a2a` |
| Violet | `#9e7ec9` | `#6a4a9a` |
| Slate | `#7a8fa0` | `#4a6070` |

The selection is persisted to `localStorage` under `nexus_accent` and applied via a `data-accent` attribute on the `<html>` element. All components that use `var(--accent)` respond immediately — tab underlines, category borders, the clock, modal headings, buttons, the fidget widget.

---

## Data & Portability

Your bookmarks live in `data.json` alongside `index.html`. The scratch pad content is also saved here when you hit Save. The format is:

```json
{
  "categories": [
    {
      "id": "cat1",
      "name": "Favorites",
      "open": true,
      "pinned": true,
      "color": "#c8a96e",
      "links": [
        { "id": "l1", "name": "Example", "url": "https://example.com" }
      ]
    }
  ],
  "scratch": "Optional scratch pad content"
}
```

RSS feeds, weather location, market cache, accent colour, theme, and search engine preference are all stored in `localStorage` only — they don't sync to GitHub. This is intentional: these are per-browser preferences, not portable data.

---

## Token Renewal

GitHub tokens expire based on the duration you set. When yours is close to expiring, GitHub will send a reminder email. To update:

1. Generate a new token at [github.com/settings/tokens](https://github.com/settings/tokens)
2. Go to **Settings → Data** in the dashboard
3. Click the GitHub sync status indicator
4. Paste the new token and save

---

## Troubleshooting

**Save failed: "does not match"**
Happens when `data.json` was updated from another browser or device, leaving the dashboard holding a stale SHA. The current version detects this automatically, fetches the correct SHA, and retries. A hard refresh (`Cmd+Shift+R` / `Ctrl+Shift+R`) will also re-sync on load.

**Bookmarks not loading / showing cached data**
The dashboard falls back to a local cache if the GitHub API is unreachable. Check the sync indicator in **Settings → Data** — click it to verify your token is valid. Tokens expire based on the duration set when created (90 days recommended).

**Weather not showing**
Go to **Settings → Data → Weather Location** and set a city. If you used GPS before, try searching by city name instead — browser geolocation can be blocked by Firefox's strict settings. Check that Open-Meteo (`api.open-meteo.com`) isn't blocked by a content blocker.

**Markets showing dashes**
Both Yahoo Finance (via corsproxy.io) and Alpha Vantage failed. This is usually a network or proxy issue. Try refreshing — corsproxy.io can be intermittently slow. Markets data is cached, so a recent value will show even if the fetch fails.

**Import only showing one folder**
Use a direct export from the browser itself (File → Export Bookmarks in Safari, or Manage Bookmarks → Export in Firefox) rather than a file that's been edited or re-saved.

**RSS feed not loading**
The dashboard tries two proxies (rss2json and corsproxy.io) before giving up. Verify the URL is correct and publicly accessible. Some feeds behind hard paywalls or with aggressive CORS restrictions cannot be fetched from a static site.

**News Brief: "Failed to generate brief: model: …"**
The Claude model string in the Worker call is outdated. Find `const BRIEF_MODEL` near the top of the `<script>` block in `index.html` and update it to the current model string (e.g. `claude-sonnet-4-6`). This is the most likely cause after Anthropic retires an older model name.

**News Brief: "Load failed" or Worker error**
Verify your Cloudflare Worker is deployed and that `ALLOWED_ORIGIN` in the Worker matches your GitHub Pages domain exactly. Check the Worker URL in `index.html` is correct. If your Anthropic API credits are exhausted, check usage at [console.anthropic.com](https://console.anthropic.com).

**Favicons not loading**
Fetched from Google's favicon service. Cosmetic only — doesn't affect functionality.

**GitHub Pages showing old version after update**
Pages can take a minute or two after a push. Wait a moment and do a hard refresh.

---

## Roadmap

- [ ] News Brief — alien observer rework: detached anthropological voice, field-notes format, naturally avoids individual names
- [ ] News Brief — TTS via ElevenLabs API for GalNet-style audio delivery
- [ ] Cloudflare Pages + Access — migrate to private hosting with email OTP or Google auth
- [ ] Nexus Lite — update the public stripped-down version on the secondary GitHub account
- [ ] LCARS interface — standalone `nexus-lcars.html`, Star Trek-inspired alternative UI
- [ ] Qualia — Reflect companion tool; processes exported `.md` files; visualises element frequency and response patterns over time
- [ ] Flow — card meditation practice; canonical word list in `data.json`; Draw/Flow mode toggle in the Prompt applet

---

## Stack

No frameworks. No build step. No package manager.

- Vanilla HTML, CSS, JavaScript
- [Syne](https://fonts.google.com/specimen/Syne) + [Inconsolata](https://fonts.google.com/specimen/Inconsolata) via Google Fonts
- [GitHub Contents API](https://docs.github.com/en/rest/repos/contents) for read/write
- [Open-Meteo](https://open-meteo.com) for weather (no key required)
- [Open-Meteo Geocoding](https://open-meteo.com/en/docs/geocoding-api) for location search (no key required)
- [Yahoo Finance](https://finance.yahoo.com) + [Alpha Vantage](https://www.alphavantage.co) for market data
- [Anthropic API](https://anthropic.com) via Cloudflare Worker proxy for News Brief
- [corsproxy.io](https://corsproxy.io) as CORS proxy for RSS feeds and market data
- [rss2json](https://rss2json.com) as primary RSS proxy

---

*Built with [Claude](https://claude.ai) — AI-assisted development at its finest.*
