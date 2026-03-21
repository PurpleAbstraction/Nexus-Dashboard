# Nexus Dashboard

A self-hosted personal dashboard and bookmark manager. Lives in your GitHub repo, works in any browser, on any device — no accounts, no sync services, no third-party dependencies for your data.

![Theme](https://img.shields.io/badge/theme-dark%20%2F%20light-c8a96e) ![Storage](https://img.shields.io/badge/storage-GitHub%20JSON-7eb8c9) ![Dependencies](https://img.shields.io/badge/dependencies-none-6ec98a) ![Mobile](https://img.shields.io/badge/layout-responsive-c96eb4)

---

## Layout

Nexus uses a three-tab interface with a persistent header. The header contains the logo, clock, and a unified search omnibar with a Links/Web mode toggle. Everything else lives in one of the three tabs.

### Tab 1 — Bookmarks
Full-width bookmark manager. Categories, links, search — everything you'd expect.

### Tab 2 — Workspace
A 2×2 panel grid (collapses to single column on mobile):
- **Web Search** — DuckDuckGo, Perplexity, or Brave, engine remembered between sessions
- **Applets** — Writing Prompt, Fidget widget, Reflect journal, Morning Brief (coming soon)
- **Scratch Pad** — Quick notes, auto-saved locally, syncs to GitHub with the Save button
- **RSS Feeds** — Up to 10 feeds, merged chronological stream, per-feed filter

### Tab 3 — Settings
Two-column layout (collapses to single column on mobile):
- **Appearance** — Dark/light mode toggle, global accent colour picker
- **Data** — Import bookmarks, export bookmarks, GitHub sync status and token management

---

## Features

### Bookmarks
- **Collapsible categories** — color-coded left border accents, favicons, instant search
- **Pinned category** — pin any one category as a permanent anchor at the top; always open, visually distinct with a ★ PINNED badge
- **Category colors** — inline color picker when renaming; choose from 30 curated colors or reset to default
- **Inline editing** — rename categories and pick colors in one step; add, edit, and delete links without touching code
- **Move links** — reassign any link to a different category via the edit modal
- **Global quick-add** — add a link without expanding any category first
- **Alphabetical sort** — sort all categories and their links A–Z in one click
- **Drag-and-drop reordering** — drag categories into any order; drag links within a category to reorder them
- **GitHub-backed storage** — bookmarks saved to `data.json` via the GitHub API with live sync status; auto-retries on SHA conflicts

### Appearance
- **Dark / light theme** — warm dark and parchment light modes, toggled from Settings
- **Global accent colour** — five choices (Gold, Teal, Ember, Violet, Slate) applied dashboard-wide; persisted to `localStorage` under `nexus_accent`
- **Responsive layout** — works on desktop, tablet, and mobile

### Import & Export
- **Browser import** — import from Safari or Firefox HTML exports with full preview: rename categories, merge into existing ones, skip duplicates
- **Export** — download a clean `nexus-data.json` backup any time; both accessible from the Settings tab

### Web Search
- **Three engines** — DuckDuckGo, Perplexity, and Brave Search, switchable with one click
- **DDG optimized** — no AI answers, no ads, dark mode, and privacy settings baked into the URL
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
- **Reflect** — daily journal with structured prompts
- **Morning Brief** — coming soon; AI-synthesized daily news brief via Claude API

### General
- **Single file** — everything in one `index.html`, no build tools, no frameworks, no dependencies
- **Custom favicon** — inline SVG node-and-spokes design in dashboard gold
- **Clock** — live date and time in the header

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

## Migrating from an earlier version

If you were using a previous version of Nexus that stored data in `bookmarks.json`, you need to rename that file before deploying this version:

1. In your GitHub repo, open `bookmarks.json`
2. Click the pencil (edit) icon
3. Change the filename at the top of the editor from `bookmarks.json` to `data.json`
4. Commit the change

Then deploy the new `index.html`. The dashboard will find `data.json` and all your existing bookmarks will load normally.

---

## Importing Bookmarks

Click **Import** in the Settings tab and drop your browser's bookmark export file:

- **Safari:** File → Export Bookmarks
- **Firefox:** Bookmarks → Manage Bookmarks → Import and Backup → Export Bookmarks to HTML

The import preview lets you rename categories, merge them into existing ones, and skip duplicates before committing anything. Nested subfolders are flattened automatically.

---

## Web Search

The web search panel in the Workspace tab supports three engines:

| Engine | Notes |
|--------|-------|
| **DuckDuckGo** | No AI · No ads · Dark mode · Private — all baked into the search URL |
| **Perplexity** | AI-powered answers · Account settings carry over when logged in |
| **Brave Search** | Independent index · Reliable fallback |

Your last-used engine is remembered between sessions.

---

## RSS Feeds

Add up to 10 feeds via the **Edit** button in the RSS panel header. Feed names are auto-generated from the hostname. Click any feed name in the list to filter to that feed; click again to return to the combined view. All feeds are merged chronologically and filtered to the last 7 days by default.

Feed URLs are stored in `localStorage` — they persist across sessions in the same browser but are not synced to GitHub. This is intentional: RSS preferences are quick to re-enter and don't need version control.

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

The selection is persisted to `localStorage` under `nexus_accent` and applied via a `data-accent` attribute on the `<html>` element. All components that use `var(--accent)` — tab underlines, category borders, modal headings, buttons, the fidget widget — respond immediately.

---

## Data & Portability

Your bookmarks live in `data.json` alongside `index.html`. You can edit it directly, back it up, or use it with other tools. The scratch pad content is also saved into this file when you hit Save. The format is:

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

---

## Token Renewal

GitHub tokens expire based on the duration you set. When yours is close to expiring, GitHub will send you a reminder email. To update:

1. Generate a new token at [github.com/settings/tokens](https://github.com/settings/tokens)
2. Go to **Settings → Data** in the dashboard
3. Click the GitHub sync status button
4. Paste the new token and save

---

## Troubleshooting

**Save failed: "does not match"**
This happens when `data.json` was updated in GitHub from another browser or device, leaving the dashboard holding a stale SHA. The current version detects this automatically, silently fetches the correct SHA, and retries the save. If you're on an older version, a hard refresh (`Cmd+Shift+R` / `Ctrl+Shift+R`) will re-sync the SHA on load.

**Bookmarks not loading / showing cached data**
If the dashboard can't reach the GitHub API (network issue, expired token), it falls back to a locally cached copy. Check the GitHub sync button in **Settings → Data** — if it shows an error, click it to verify your token is still valid.

**Import only showing one folder**
Make sure you're using a direct export from the browser itself (File → Export Bookmarks in Safari, or Manage Bookmarks → Export in Firefox) rather than a file that's been edited or re-saved.

**RSS feed not loading**
The dashboard tries two proxies (rss2json and corsproxy.io) before giving up. If a feed still fails, verify the URL is correct and publicly accessible. Some feeds behind hard paywalls or with aggressive CORS restrictions cannot be fetched from a static site.

**Favicons not loading**
Favicons are fetched from Google's favicon service. If a favicon doesn't appear, the site either doesn't have one or the request was blocked. Cosmetic only.

**GitHub Pages showing old version after update**
GitHub Pages can take a minute or two to reflect changes after a push. Wait a moment and do a hard refresh (`Cmd+Shift+R` / `Ctrl+Shift+R`).

---

## Roadmap

- [ ] Morning Brief panel — AI-synthesized daily news brief from wire services via Claude API, with per-item Perplexity deep-dive links
- [ ] Daily Reflection companion tool — processes exported `.md` files, maps data points, charts trends over time

---

## Stack

No frameworks. No build step. No package manager.

- Vanilla HTML, CSS, JavaScript
- [Syne](https://fonts.google.com/specimen/Syne) + [Inconsolata](https://fonts.google.com/specimen/Inconsolata) via Google Fonts
- [GitHub Contents API](https://docs.github.com/en/rest/repos/contents) for read/write
- [rss2json](https://rss2json.com) + [corsproxy.io](https://corsproxy.io) as CORS proxies for RSS feeds

---

*Built with [Claude](https://claude.ai) — AI-assisted development at its finest.*
