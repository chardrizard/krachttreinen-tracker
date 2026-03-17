# 🏋️ Gym Progress Tracker

A minimal, mobile-first PWA for tracking progressive overload at the gym. Built for speed and simplicity — log your sets in seconds, track your gains over time.

![Gym Tracker Screenshot](screenshot.png)

## ✨ Features

- **Quick Logging** — Tap to increment weight (+2.5kg), enter reps, done
- **Progressive Overload Tracking** — See your starting weight, current weight, and % progress
- **Last Session Reference** — Always know what you lifted last time
- **Offline-First** — Works without internet after first load
- **No Account Required** — All data stays on your device (localStorage)

## 📱 Install on iPhone

1. Open the app in Safari
2. Tap the **Share** button (square with arrow)
3. Scroll down and tap **"Add to Home Screen"**
4. Name it and tap **Add**

The app will appear on your home screen with a custom icon and run in full-screen mode.

## 🚀 Deploy to GitHub Pages

1. Fork this repository
2. Go to **Settings → Pages**
3. Under "Source", select **Deploy from a branch**
4. Choose `main` branch and `/ (root)` folder
5. Click **Save**

Your app will be live at `https://yourusername.github.io/gym-tracker/`

## 📂 Project Structure

```
gym-tracker/
├── index.html      # Complete app (single file)
├── manifest.json   # PWA configuration
├── sw.js          # Service worker for offline support
├── icons/
│   ├── icon-192.png
│   ├── icon-512.png
│   └── apple-touch-icon.png
└── README.md
```

## 🎯 Usage

### Logging a Workout

1. Tap **Log Exercise** or select from recent exercises
2. Choose an exercise from the list (or create custom)
3. Use **−** / **+** buttons to adjust weight
4. Enter reps and tap **Add Set**
5. Repeat for each set
6. Tap **Save** when done

### Viewing Progress

1. Go to **Progress** tab
2. Select an exercise from the dropdown
3. Choose time range (30 days / 90 days / All time)
4. View your progression chart and stats

### Settings

- **Weight Increment** — Change the +/- step (1.25, 2.5, or 5 kg)
- **Weight Unit** — Switch between kg and lbs
- **Export/Import** — Backup your data as JSON
- **Clear Data** — Start fresh (irreversible)

## 🛠 Tech Stack

- **Vanilla JavaScript** — No frameworks, no build step
- **Chart.js** — For progression charts
- **localStorage** — Client-side data persistence
- **Service Worker** — Offline caching

## 📊 Data Schema

All data is stored in `localStorage` under `gym-tracker-data`:

```javascript
{
  exercises: [...],      // Exercise definitions
  logs: [...],           // Workout logs with sets
  settings: {
    weightIncrement: 2.5,
    weightUnit: 'kg',
    theme: 'dark'
  },
  recentExercises: [...]  // Last 10 used exercise IDs
}
```

## 🔒 Privacy

- **No tracking** — Zero analytics or telemetry
- **No accounts** — No server, no sign-up
- **Local only** — Your data never leaves your device
- **Export anytime** — Full data portability via JSON export

## 📄 License

MIT — Use it, modify it, ship it.

---

Built with 💪 for lifters who want to track gains, not fiddle with apps.
