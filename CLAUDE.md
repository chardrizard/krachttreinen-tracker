# CLAUDE.md — Liftd

## What this project is

Mobile-first PWA for tracking progressive overload at the gym. Powerlifting-focused, offline-first, no account. Personal use.

Deployed: https://chardrizard.github.io/liftd/

## File structure

```
/
├── index.html      — entire app (HTML + CSS + JS inline)
├── manifest.json
├── sw.js           — service worker (offline caching)
├── favicon.svg
├── icons/          — PWA icons (apple-touch, 192, 512)
└── image/          — screenshots
```

## Architecture

- Single `index.html`, all inline. Chart.js loaded from CDN for progression charts. Service worker for offline caching.
- All data in `localStorage` across 4 keys:

```js
gym_exercises    // exercise definitions + custom + aliases
gym_logs         // { id, date, timestamp, exerciseId, sets[] }
gym_settings     // { weightIncrement, weightUnit, theme }
gym_favourites   // [exerciseId]
```

## Core features

- Log session (preset or custom exercises, set weight/reps).
- Favourites carousel + latest-session quick re-log on home.
- Edit / delete today's entries (delete = confirmation modal).
- Progression chart — estimated 1RM via Epley: `weight × (1 + reps/30)`.
- 4 explanation states: first session, progressing, declining, plateaued.
- Auto-select most recently logged exercise on Progress tab.
- Export / import JSON.
- Configurable weight increment (1.25 / 2.5 / 5 kg), unit (kg / lbs).

## Design system

```css
--surface-base: #0a0a0a / --surface-raised: #141414 / --surface-overlay: #1a1a1a
--text-primary: #fafafa / --text-secondary: #a1a1a1
--accent: #22c55e            /* green — only accent */
--destructive: #ef4444 / --warning: #f59e0b
font: Inter (body) + DM Mono (data/labels)
```

## Preset exercises

~40 across chest, back, shoulders, arms, legs, compound, core, glutes — `PRESET_EXERCISES` array in `index.html`. Custom exercises stored separately.

## Hard rules

- Single `index.html` — don't split CSS/JS into separate files.
- No npm, no build step, no framework.
- Chart.js is the **only** CDN dependency.
- Exercise shape: `{ id, name, alias, category, icon, isCustom }`.
- Log shape: `{ id, date (YYYY-MM-DD), timestamp, exerciseId, sets: [{weight, reps}] }`.
- Modifying `sw.js`: **always bump `CACHE_NAME`** to today's date — `'liftd-YYYY-MM-DD'`. Stale caches break the PWA on reinstall.
- Destructive actions (delete entry, clear all data) always require confirmation.
- No `alert()` — use the existing modal system.

## Common edit pointers

- **Add preset exercise:** append to `PRESET_EXERCISES` (`id`, `name`, `category`, `icon`).
- **1RM chart config:** `renderProgressChart()`.
- **1RM explanation states:** `render1RMExplanation()`.
- **Deploy:** push to `main` (auto-deploys). Bump `CACHE_NAME` in `sw.js` first.
