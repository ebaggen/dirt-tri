# Dirt Tri

Offline-first triathlon leg-time tracker. Runs as a PWA on an iPad — no internet
required during the race. Built as a single static page (no build step, no
framework).

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The entire app (HTML, CSS, JS inlined) |
| `sw.js` | Service worker that pre-caches everything for offline use |
| `manifest.webmanifest` | PWA manifest (app name, icon, theme) |
| `icon-512.png` | Home-screen icon |
| `icon.svg` | Source SVG for the icon (not used at runtime — replace `icon-512.png` if you want a different icon) |

## Hosting on GitHub Pages

1. Create a new repo on GitHub (e.g. `dirt-tri`).
2. Push these files to the `main` branch:
   ```sh
   cd /Users/ebaggen/dev/dirt-tri
   git init
   git add .
   git commit -m "Initial Dirt Tri PWA"
   git branch -M main
   git remote add origin https://github.com/<your-account>/dirt-tri.git
   git push -u origin main
   ```
3. On GitHub: **Settings → Pages → Build and deployment → Source: Deploy from
   branch → Branch: `main` / root → Save**.
4. Wait ~1 min. Your URL is `https://<your-account>.github.io/dirt-tri/`.

## Installing on the iPad (do this once, before race day, with internet)

1. Open Safari on the iPad.
2. Go to your GitHub Pages URL.
3. Let the page fully load (the service worker caches all assets on first
   visit). Reload once to make sure nothing is missing from the cache.
4. Tap the **Share** button → **Add to Home Screen** → confirm name "Dirt Tri".
5. Open the app from the new home-screen icon. It now works fully offline.

### Race-day iPad settings

- **Settings → Display & Brightness → Auto-Lock → Never** (belt-and-suspenders;
  the app also uses the Wake Lock API while a race is active).
- **Brightness: max** for outdoor visibility.
- **Settings → Safari → Advanced → Website Data** — do **not** clear it; that
  wipes the app's race data.

### Updating the app

When you push changes to `main`, the iPad won't auto-update (it's offline).
With internet, open the home-screen app, pull to refresh in Safari, and the
service worker fetches the new version on next launch.

## Usage

- **Setup**: enter a race name, add Solo participants and/or Team entries (just
  names). Tap **START RACE** for a 3-2-1 countdown that locks the start time.
- **During race**: search by name or browse the sections (On the swim / On the
  bike / On the run / Finished). Tap a participant's tile to record their next
  leg time. A 5-second UNDO toast appears for fat-fingers.
- **Menu** (top-right): **Manage participants** (add late, mark DNF, remove),
  **Edit times** (manual corrections), **Download backup** (JSON), **End race**.
- **After the race**: two leaderboards (Solo / Team), **Download CSV** /
  **Copy as text** / **Share / AirDrop**. **Archive & start new race** to clear
  the board for next time.
- **Past races** (from the setup screen): view or re-export old results.

## Data storage

All data lives in IndexedDB on the iPad. The **Download backup** button writes
a JSON snapshot to the Files app, and **Restore from backup** reads it back.
Take a backup any time you want a safety copy.

## Disposable

Throwaway code by design — no tests, no build step, no maintenance plan. Edit
`index.html` directly; refresh in Safari to see changes.
