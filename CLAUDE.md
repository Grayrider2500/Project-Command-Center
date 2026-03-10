# CLAUDE.md

This file provides guidance to Claude Code when working with code in this repository.

## Project Overview

**Project Command Center** is a standalone single-file web app and PWA for managing projects and schedules. It is a self-contained `.html` file with embedded CSS and JavaScript — no build step, no dependencies (except Google Fonts for typography), no server required.

## Architecture Pattern

Single-file app with all CSS, HTML, and JS embedded in `project-command-center.html`.

### Pages

| Page | Description |
|---|---|
| Dashboard | Project cards with priority scoring, status tracking, and progress |
| Schedule | Day timeline with time blocks + mini calendar |

### Data Storage

All data is stored in `localStorage` under these keys:

| Key | Contents |
|---|---|
| `pcc4_proj` | Projects array |
| `pcc4_blk` | Time blocks array |
| `pcc4_cond` | Condensed/collapsed UI state |

### CSS Theme

Dark techy UI using CSS custom properties:

```css
:root {
  --bg: #0d0e10;
  --surface: #16181e;
  --surface2: #1e2229;
  --surface3: #252b34;
  --border: #2e3545;
  --accent: #e8a020;   /* amber */
  --accent2: #4a9eff;  /* blue */
  --accent3: #3ecf8e;  /* green */
  --danger: #e05252;
  --purple: #a78bfa;
}
```

Typography uses Google Fonts: Share Tech Mono, Barlow Condensed, Barlow. Falls back to system fonts offline.

## PWA Setup

| File | Purpose |
|---|---|
| `manifest-cc.json` | PWA manifest (name, icons, theme, start_url) |
| `icon-cc.svg` | Home screen icon (dark theme, grid/calendar/progress design) |
| `sw.js` | Service worker — network-first for HTML, cache-first for other assets |

**Cache version string**: `command-center-v1` in `sw.js`. Bump this string (e.g. `v1` → `v2`) whenever deploying changes so iOS devices pick up the update.

**iOS cache clearing** (if a device is stuck on an old version):
1. Delete the home screen icon
2. Settings → Safari → Clear History and Website Data
3. Re-add to home screen from browser

## GitHub Workflow

- Deployed via GitHub Pages
- After completing any meaningful set of changes, commit and push to GitHub
- Use clear, descriptive commit messages summarizing what changed

## Conventions

- No required external dependencies — works fully offline (Google Fonts degrades gracefully)
- `font-size: 16px` on all form inputs (prevents iOS auto-zoom)
- Use `crypto.randomUUID()` for IDs
- All `localStorage` calls wrapped in `try/catch`
- HTML is escaped before inserting user content into the DOM
