# Nexus Dashboard

A self-hosted personal dashboard and bookmark manager. Lives in your GitHub repo, works in any browser, on any device — no accounts, no sync services, no third-party dependencies for your data.

![Theme](https://img.shields.io/badge/theme-dark%20%2F%20light-c8a96e) ![Storage](https://img.shields.io/badge/storage-GitHub%20JSON-7eb8c9) ![Dependencies](https://img.shields.io/badge/dependencies-none-6ec98a) ![Mobile](https://img.shields.io/badge/layout-responsive-c96eb4)

---

## Features

### Bookmarks
- **Collapsible categories** — color-coded left border accents, favicons, instant search with Clear button
- **Pinned category** — pin any one category as a permanent anchor at the top; always open, visually distinct with a ★ PINNED badge
- **Category colors** — inline color picker when renaming; choose from 30 curated colors or reset to default
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
- **Feed counter** — live `n / 10` indicator in the panel header; turns red at the limit
- **Combined view** — all feeds merged into a single chronological stream with source labels
- **Per-feed filter** — dropdown to view any single feed in isolation
- **Inline rename** — click any feed name in Edit mode to rename it in place
- **Drag-to-reorder** — drag feeds into any order within the Edit panel
- **7-day filter** — items older than 7 days are automatically hidden
- **Dual proxy** — tries rss2json first, falls back to corsproxy.io with native XML parsing; supports both RSS and Atom formats
- **Refresh button** — reload all feeds on demand

### Scratch Pad
- **Auto-save** — keystrokes saved to localStorage instantly, debounced 800ms
- **GitHub sync** — Save button pushes pad content into `bookmarks.json` alongside your bookmarks; syncs across devices on next load
- **Clear button** — wipes the pad with confirmation

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

The import preview lets you rename categories, merge them into existing ones, and skip duplicates before committing anything. Nested subfolders are flattened automatically.

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

Add up to 10 feeds via the **Edit** button in the RSS panel header. Feed names are auto-generated from the hostname and can be renamed inline by clicking the name while in Edit mode. Drag feeds to reorder them. Click any feed name outside Edit mode to filter to that feed; click again to return to the combined view.

Feed URLs are stored in browser localStorage — they persist across sessions but are not synced to GitHub. This is intentional: RSS preferences are quick to re-enter and don't need version control.

---

## Scratch Pad

The scratch pad auto-saves to localStorage as you type. To sync it across devices, press the **Save** button — this writes the pad content into `bookmarks.json` on GitHub alongside your bookmarks. The next time you load the dashboard on any device, the saved content will be restored automatically.

---

## Data & Portability

Your bookmarks live in `bookmarks.json` alongside `index.html`. You can edit it directly, back it up, or use it with other tools. The format is simple:

```json
{
  "scratch": "Your scratch pad content here.",
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
This happens when `bookmarks.json` was updated from another browser or device, leaving the dashboard holding a stale SHA. The current version detects this automatically, silently fetches the correct SHA, and retries. If you're on an older version, a hard refresh (`Cmd+Shift+R` / `Ctrl+Shift+R`) will re-sync on load.

**Bookmarks not loading / showing cached data**
If the dashboard can't reach the GitHub API, it falls back to a locally cached copy. Check the **● GitHub** indicator — if it shows an error, click it to verify your token is still valid.

**Import only showing one folder**
Make sure you're using a direct export from the browser itself (File → Export Bookmarks in Safari, or Manage Bookmarks → Export in Firefox) rather than a file that's been edited or re-saved.

**RSS feed not loading**
The dashboard tries two proxies before giving up. If a feed still fails, verify the URL is correct and publicly accessible. Some feeds behind hard paywalls or with aggressive CORS restrictions cannot be fetched from a static site.

**Favicons not loading**
Favicons are fetched from Google's favicon service. If one doesn't appear, it's cosmetic only and doesn't affect functionality.

**GitHub Pages showing old version after update**
GitHub Pages can take a minute or two to reflect changes. Wait a moment and do a hard refresh (`Cmd+Shift+R` / `Ctrl+Shift+R`).

---

## Roadmap

- [ ] Morning Brief panel — AI-synthesized daily news brief from wire services (AP, Reuters, AFP), rendered neutral and fact-based via Claude API, with per-item Perplexity deep-dive links
- [ ] Global search toggle — unified header search bar switchable between bookmark filtering and web search; omnibar with live bookmark results and a "Search the web for…" escape hatch

---

## Stack

No frameworks. No build step. No package manager.

- Vanilla HTML, CSS, JavaScript
- [Syne](https://fonts.google.com/specimen/Syne) + [Inconsolata](https://fonts.google.com/specimen/Inconsolata) via Google Fonts
- [GitHub Contents API](https://docs.github.com/en/rest/repos/contents) for read/write
- [rss2json](https://rss2json.com) + [corsproxy.io](https://corsproxy.io) as CORS proxies for RSS feeds

---

*Built with [Claude](https://claude.ai) — AI-assisted development at its finest.*
