# CalorieTracker — PWA

Miami Vice calorie + mobility tracker as a Progressive Web App. Same features as the native iOS app, runs in Safari, installable to your home screen, **barcode scanner included**.

## What's in here

```
CalorieTracker-PWA/
├── index.html              # The entire app (Preact + htm + ZXing via CDN)
├── manifest.webmanifest    # PWA manifest (makes it installable)
├── sw.js                   # Service worker (offline + install)
├── icon-192.png            # App icon (small)
├── icon-512.png            # App icon (large)
└── README.md               # This file
```

No build step. No node_modules. Open in any browser, host on any static host.

---

## ⚡️ FASTEST PATH TO BARCODE SCANNER ON YOUR PHONE (2 minutes)

iOS Safari **requires HTTPS** for camera access. So `http://localhost` works on your Mac, but not on your phone. The 30-second fix: drop the folder on Netlify.

1. Open <https://app.netlify.com/drop> in your browser.
2. Drag the **entire `CalorieTracker-PWA` folder** onto the upload zone.
3. Netlify gives you a URL like `https://radiant-sunset-1a2b3c.netlify.app`. **Open that on your iPhone in Safari.**
4. In Safari, tap the **Share** icon (square with arrow) → **Add to Home Screen** → Add.
5. Tap the new icon on your home screen — it opens fullscreen like a real app.
6. Tap the pink barcode glyph in the search bar → grant camera permission → scan.

**That's it. No Apple Developer account, no $99, no Xcode, no 7-day expiry.** Lives on your phone forever as long as Netlify keeps the URL up (which they do for free).

If you want a custom URL (e.g. `tracker.yourname.com`), you can sign up for a free Netlify account and add a domain. Otherwise the auto-generated URL works fine.

---

## Other deployment options

### GitHub Pages (free, custom URL via github.io)
```bash
cd /Users/colin/Desktop/Code/CalorieTracker-PWA
git init
git add -A
git commit -m "PWA"
gh repo create calorietracker-pwa --public --source=. --push
gh repo edit --enable-pages --pages-branch main --pages-path /
```
Then open `https://YOUR_GITHUB_USERNAME.github.io/calorietracker-pwa/` on your phone.

### Cloudflare Pages (free, fast)
1. Sign in at <https://pages.cloudflare.com>.
2. Connect a GitHub repo (push the folder first like above), or use the "Direct Upload" option.
3. Get an HTTPS URL.

### Local development (Mac only — phone won't work for camera)
```bash
cd /Users/colin/Desktop/Code/CalorieTracker-PWA
python3 -m http.server 8765
```
Then visit `http://localhost:8765/` in **Safari or Chrome on your Mac**.
- Barcode scanner works on Mac (uses your laptop's camera)
- Barcode scanner will NOT work over LAN to your iPhone (iOS requires HTTPS)

---

## Features

- ✅ **Diary** — log meals across Breakfast/Lunch/Dinner/Snacks. Calorie ring + macro bars. Date navigation.
- ✅ **Search** — curated common foods (top ~40), USDA FoodData Central API, your own history (frequency-ranked Top Foods grid)
- ✅ **Barcode scanner** — uses ZXing-js + camera. Looks up Open Food Facts + USDA in parallel.
- ✅ **Mobility checklist** — 50 pull-ups / 100 push-ups / 100 squats / 10 min mobility. Streak counter + week dots + stats.
- ✅ **Progress** — weight trend line chart (cyan→pink gradient), calorie bar chart (under/over goal), stat tiles.
- ✅ **Settings** — goal editor (calories + macros, ± steppers), weight log, target weight, weight unit (lb/kg), reset.
- ✅ **Road House celebration** — when you log an approved food. Same Yeah Buddy stamp.
- ✅ **Installable** — Safari → Share → Add to Home Screen makes it fullscreen and offline-capable.
- ✅ **Offline** — service worker caches the app shell; works without internet (search needs internet, everything else doesn't).

## Trade-offs vs the native iOS app

| Feature | Native | PWA |
|---|---|---|
| Barcode scanner | VisionKit (instant) | ZXing-js (~100ms) — still fast and accurate |
| Persistence | SwiftData | localStorage |
| Camera permission | Native prompt | Safari prompt |
| Apple Health | Yes | No (Web has no HealthKit) |
| Push notifications | Native | iOS PWA push works as of iOS 16.4 (not wired up yet) |
| Install | TestFlight/AppStore + $99/yr | Add to Home Screen — free, forever |
| Renewal | 7 days (free) / 1 yr (paid) | Never expires |

## How your data is stored

Everything is in **localStorage** under the key `ct-pwa-v1`. It stays on your phone, never leaves. If you want to back it up, open the app on desktop Safari and run this in DevTools:
```js
copy(localStorage.getItem('ct-pwa-v1'))
```
Or in Settings → "Reset data" to wipe.

## API keys

Uses the same USDA FoodData Central API key as the native app (publicly distributable, rate-limited). Open Food Facts is unauthenticated.

## Notes

- First barcode scan loads the ZXing library from a CDN (~100KB, then cached forever via service worker).
- Camera permission is per-origin in Safari. Granting it on the Netlify URL grants it for that specific URL.
- If you reset/clear your browser data, the app forgets everything (and you'll need to re-grant camera).
- "Add to Home Screen" PWAs on iOS persist their data unless you manually delete the home-screen icon.

---

Built to match the native app's Miami Vice design exactly, using only standard web tech (no React build step, no TypeScript, no bundler).
