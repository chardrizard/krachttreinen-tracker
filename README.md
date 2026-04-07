# 🏋️ Gym Progress Tracker

A minimal, mobile-first PWA for tracking progressive overload at the gym. Built for speed and simplicity — log your sets in seconds, track your gains over time.

![Gym Tracker Screenshot](image/Screenshot%202026-04-07%20at%2010.19.35.png)

## ✨ Features

- **Quick Logging** — Tap to increment weight (+2.5kg), enter reps, done
- **Edit Today's Entries** — Tap any entry logged today to correct sets, weights, or reps
- **Latest Session Reference** — Home screen always shows your last workout day, tappable to re-log any exercise
- **1RM-Based Progress** — Progress chart and stats use estimated 1-rep max (Epley formula), so adding reps counts as improvement
- **Last Session Reference** — Always know what you lifted last time before logging
- **Remove Last Set** — Add or remove sets during logging without misclicking
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

1. Tap **Log Exercise** or select from favourites / latest session
2. Choose an exercise from the list (or create custom)
3. Use **−** / **+** buttons to adjust weight
4. Enter reps — use **Add Set** / **Remove Last Set** to manage sets
5. Tap **Save** when done

### Editing a Today Entry

1. From the Home screen, tap any entry under **Today**
2. Adjust sets, weights, or reps as needed
3. Tap **Save** — the original log is updated in place
4. To remove the entry entirely, tap **Delete This Entry** (confirmation required)

### Logging from Last Session

- The **Latest** section on the home screen shows your most recent past workout
- Tap any exercise to open a new log pre-filled with last session's sets

### Viewing Progress

1. Go to **Progress** tab
2. Select an exercise from the dropdown
3. Choose time range (30 days / 90 days / All time)
4. Chart shows estimated 1RM per session — both weight and rep increases are reflected

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

All data is stored in `localStorage` across four keys:

```javascript
gym_exercises   // Exercise definitions + aliases
gym_logs        // Workout logs: { id, date, timestamp, exerciseId, sets[] }
gym_settings    // { weightIncrement, weightUnit, theme }
gym_favourites  // Array of favourited exercise IDs
```

## 📐 Progress Calculation

Progress is based on **estimated 1-rep max** using the Epley formula:

```
Est. 1RM = weight × (1 + reps / 30)
```

This means lifting 80kg × 8 reps (est. 1RM ≈ 101kg) correctly shows as better than 82.5kg × 1 rep. Both weight and rep improvements count toward progress.

## 🔒 Privacy

- **No tracking** — Zero analytics or telemetry
- **No accounts** — No server, no sign-up
- **Local only** — Your data never leaves your device
- **Export anytime** — Full data portability via JSON export

## 📄 License

MIT — Use it, modify it, ship it.

---

Built with 💪 for lifters who want to track gains, not fiddle with apps.
