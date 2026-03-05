# Nexus Dashboard

A self-hosted personal dashboard and bookmark manager. Lives in your GitHub repo, works in any browser, on any device — no accounts, no sync services, no third-party dependencies for your data.

![Theme](https://img.shields.io/badge/theme-dark%20%2F%20light-c8a96e) ![Storage](https://img.shields.io/badge/storage-GitHub%20JSON-7eb8c9) ![Dependencies](https://img.shields.io/badge/dependencies-none-6ec98a) ![Mobile](https://img.shields.io/badge/layout-responsive-c96eb4)

---

## Features

- **Bookmark manager** — collapsible categories with color-coded accents, favicons, instant search with Clear button
- **Inline editing** — rename categories, add/edit/delete links without touching code
- **Alphabetical sort** — sort all categories and their links A–Z in one click
- **GitHub-backed storage** — bookmarks saved to `bookmarks.json` in this repo via the GitHub API, with live sync status indicator
- **Browser import** — import from Safari or Firefox bookmark HTML exports with preview, inline rename, category merging, and duplicate detection
- **Web search widget** — search DuckDuckGo, Perplexity, or Brave Search directly from the dashboard; engine preference remembered between sessions; DDG launches with no AI, no ads, dark mode, and privacy settings baked in
- **RSS feed reader** — paste any feed URL, results load in the sidebar with refresh button
- **Scratch pad** — quick notes that auto-save to browser localStorage
- **Theme switcher** — warm dark and parchment light modes
- **Export** — download your bookmarks as a clean JSON file any time
- **Responsive layout** — works on desktop, tablet, and mobile
- **Single file** — everything in one `index.html`, no build tools, no frameworks, no dependencies

---

## Setup

### 1. Fork or clone this repo

Or simply upload `index.html` and `bookmarks.json` to any existing GitHub repo.

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

The import preview lets you rename categories, merge them into existing ones, and skip duplicates before committing anything.

---

## Web Search

The web search panel in the sidebar supports three engines:

| Engine | Notes |
|--------|-------|
| **DuckDuckGo** | No AI · No ads · Dark mode · Private — all baked into the search URL |
| **Perplexity** | AI-powered answers · Account settings carry over when logged in |
| **Brave Search** | Independent index · Fallback option |

Your last-used engine is remembered between sessions.

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

## Roadmap

- [ ] Drag-and-drop category and link reordering
- [ ] Multiple RSS feeds
- [ ] Game launcher integration

---

## Stack

No frameworks. No build step. No package manager.

- Vanilla HTML, CSS, JavaScript
- [Syne](https://fonts.google.com/specimen/Syne) + [Inconsolata](https://fonts.google.com/specimen/Inconsolata) via Google Fonts
- [GitHub Contents API](https://docs.github.com/en/rest/repos/contents) for read/write
- [rss2json](https://rss2json.com) as a CORS proxy for RSS feeds

---

*Built with [Claude](https://claude.ai) — AI-assisted development at its finest.*

