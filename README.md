# Nexus Dashboard

A self-hosted personal dashboard and bookmark manager. Lives in your GitHub repo, works in any browser, on any device — no accounts, no sync services, no third-party dependencies for your data.

![Theme](https://img.shields.io/badge/theme-dark%20%2F%20light-c8a96e) ![Storage](https://img.shields.io/badge/storage-GitHub%20JSON-7eb8c9) ![Dependencies](https://img.shields.io/badge/dependencies-none-6ec98a) ![Mobile](https://img.shields.io/badge/layout-responsive-c96eb4)

---

## Features

### Bookmarks
- **Collapsible categories** — color-coded left border accents, favicons, instant search with Clear button
- **Pinned category** — pin any one category as a permanent anchor at the top; always open, visually distinct with a ★ PINNED badge
- **Category colors** — inline color picker when renaming; choose from 10 curated colors or reset to default
- **Inline editing** — rename categories and pick colors in one step; add, edit, and delete links without touching code
- **Move links** — reassign any link to a different category via the edit modal
- **Global quick-add** — add a link from the header without expanding any category first
- **Alphabetical sort** — sort all categories and their links A–Z in one click
- **Drag-and-drop reordering** — drag categories into any order; drag links within a category to reorder them
- **GitHub-backed storage** — bookmarks saved to `bookmarks.json` via the GitHub API with live sync status; auto-retries on SHA conflicts

### Import & Export
- **Browser import** — import from Safari or Firefox HTML exports with full preview: rename categories, merge into existing ones, skip duplicates
- **Export** — download a clean `bookmarks.json` backup any time

### Web Search
- **Three engines** — DuckDuckGo, Perplexity, and Brave Search, switchable with one click
- **DDG optimized** — no AI answers, no ads, dark mode, and privacy settings baked into the URL
- **Persistent preference** — last used engine remembered between sessions
- **Clear button** — clears the search input instantly

### RSS Feeds
- **Up to 10 feeds** — add, remove, and manage feeds via an Edit panel that tucks away when not in use
- **Combined view** — all feeds merged into a single chronological stream with source labels
- **Per-feed filter** — dropdown to view any single feed in isolation
- **7-day filter** — items older than 7 days are automatically hidden
- **Dual proxy** — tries rss2json first, falls back to corsproxy.io with native XML parsing for maximum compatibility; supports both RSS and Atom formats
- **Refresh button** — reload all feeds on demand

### Sidebar Panels
- **Scratch pad** — quick notes that auto-save to browser localStorage with a Clear button
- **RSS feeds panel** — compact sidebar panel with Edit/Done toggle

### General
- **Theme switcher** — warm dark and parchment light modes
- **Clock** — live date and time in the header
- **Responsive layout** — works on desktop, tablet, and mobile
- **Single file** — everything in one `index.html`, no build tools, no frameworks, no dependencies
- **Custom favicon** — inline SVG node-and-spokes design in dashboard gold

---

## Setup

### 1. Fork or clone this repo
Or upload `index.html` and `bookmarks.json` to any existing GitHub repo.

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
Open your dashboard URL. On first visit, a prompt will appear asking for your token. Paste it in and hit Save. It's stored only in your browser's localStorage — never in the repo.

That's it. Every bookmark change you make will now sync silently back to `bookmarks.json` in this repo.

---

## Importing Bookmarks

Click **Import** in the header and drop your browser's bookmark export file:

- **Safari:** File → Export Bookmarks
- **Firefox:** Bookmarks → Manage Bookmarks → Import and Backup → Export Bookmarks to HTML

The import preview lets you rename categories, merge them into existing ones, and skip duplicates before committing anything. Nested subfolders are flattened automatically using your existing naming convention (e.g. `Game - Tool`).

---

## Web Search

The web search panel in the sidebar supports three engines:

| Engine | Notes |
|--------|-------|
| **DuckDuckGo** | No AI · No ads · Dark mode · Private — all baked into the search URL |
| **Perplexity** | AI-powered answers · Account settings carry over when logged in |
| **Brave Search** | Independent index · Reliable fallback |

Your last-used engine is remembered between sessions.

---

## RSS Feeds

Add up to 10 feeds via the **Edit** button in the RSS panel header. Feed names are auto-generated from the hostname. Click any feed name in the list to filter to that feed; click again to return to the combined view. All feeds are merged chronologically and filtered to the last 7 days by default.

Feed URLs are stored in browser localStorage — they persist across sessions in the same browser but are not synced to GitHub. This is intentional: RSS preferences are quick to re-enter and don't need version control.

---

## Data & Portability

Your bookmarks live in `bookmarks.json` alongside `index.html`. You can edit it directly, back it up, or use it with other tools. The format is simple:

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
  ]
}
```

---

## Token Renewal

GitHub tokens expire based on the duration you set. When yours is close to expiring, GitHub will send you a reminder email. To update:

1. Generate a new token at [github.com/settings/tokens](https://github.com/settings/tokens)
2. Click the **● GitHub** indicator in the dashboard header
3. Paste the new token and save

---

## Troubleshooting

**Save failed: "does not match"**
This happens when `bookmarks.json` was updated in GitHub from another browser or device, leaving the dashboard holding a stale SHA. The current version detects this automatically, silently fetches the correct SHA, and retries the save. If you're on an older version, a hard refresh (`Cmd+Shift+R` / `Ctrl+Shift+R`) will re-sync the SHA on load.

**Bookmarks not loading / showing cached data**
If the dashboard can't reach the GitHub API (network issue, expired token), it falls back to a locally cached copy. Check the **● GitHub** indicator in the header — if it shows an error, click it to verify your token is still valid. Tokens expire based on the duration set when created (90 days recommended).

**Import only showing one folder**
Safari and Firefox structure their bookmark HTML exports differently. If only one category appears after import, make sure you're using a direct export from the browser itself (File → Export Bookmarks in Safari, or Manage Bookmarks → Export in Firefox) rather than a file that's been edited or re-saved.

**RSS feed not loading**
The dashboard tries two proxies (rss2json and corsproxy.io) before giving up. If a feed still fails, verify the URL is correct and publicly accessible. Some feeds behind hard paywalls or with aggressive CORS restrictions cannot be fetched from a static site. Check [Feedly](https://feedly.com) or your browser to confirm the feed is live.

**Favicons not loading**
Favicons are fetched from Google's favicon service. If a favicon doesn't appear, the site either doesn't have one or the request was blocked. This is cosmetic only and doesn't affect functionality.

**GitHub Pages showing old version after update**
GitHub Pages can take a minute or two to reflect changes after a push. If you're seeing a stale version, wait a moment and do a hard refresh (`Cmd+Shift+R` / `Ctrl+Shift+R`).

---

## Roadmap

### Phase 2

**Search & Navigation**
- [ ] Global search with unified dropdown — single input that searches bookmarks or dispatches a web search, with mode toggle
- [ ] Ensure all web search engine URLs have preference parameters fully baked in (privacy, UI, etc.)

**Bookmarks & Categories**
- [ ] Improved category color picker — full spectrum or expanded palette

**RSS**
- [ ] Custom feed names — ability to rename feeds in the Edit panel rather than auto-deriving from hostname
- [ ] Surface the 10-feed cap visibly in the widget UI (currently enforced in code; add a note so it's clear to the user)

**Scratch Pad**
- [ ] GitHub-backed scratch pad for the full version — persist notes to the repo alongside `bookmarks.json` (lite version keeps localStorage as-is)

**Lite Version**
- [ ] Stripped-down single-file build for sharing with friends — no GitHub token required, no sync, all data in localStorage only; bookmarks exportable as JSON for portability

---

## Stack

No frameworks. No build step. No package manager.

- Vanilla HTML, CSS, JavaScript
- [Syne](https://fonts.google.com/specimen/Syne) + [Inconsolata](https://fonts.google.com/specimen/Inconsolata) via Google Fonts
- [GitHub Contents API](https://docs.github.com/en/rest/repos/contents) for read/write
- [rss2json](https://rss2json.com) + [corsproxy.io](https://corsproxy.io) as CORS proxies for RSS feeds

---

*Built with [Claude](https://claude.ai) — AI-assisted development at its finest.*
