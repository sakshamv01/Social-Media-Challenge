# Startup Roulette

A single-file HTML app — a "slot machine" that spins up a random startup idea
(industry + customer + constraint) for students, gives them a chance to keep
their first result or spin once more, then runs a 10-minute submission timer.
Includes an organizer/admin panel with a live overdue stopwatch and CSV export.

Everything lives in one file: `index.html`. No build step, no dependencies.

## Data storage

Student and submission data is stored via a small `window.storage` shim built
into the page:
- If a native `window.storage` already exists (e.g. when this file is opened
  as a **published Claude artifact**), that's used automatically.
- Otherwise (self-hosted, or opened as a local file) it falls back to a
  **Google Apps Script Web App** backed by a Google Sheet, for shared data,
  and the browser's `localStorage` for personal/per-device data.

The Apps Script URL is set near the top of the `<script>` block:

```js
const SCRIPT_URL = "https://script.google.com/macros/s/AKfycby.../exec";
```

Replace this with your own deployed Apps Script Web App URL if you fork this.
Your Apps Script deployment must be set to **"Who has access: Anyone"**, or
students on other devices/browsers will get permission errors.

## Run it locally

No server needed — just open the file:

```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
open index.html      # macOS
# or: start index.html   (Windows)
# or: xdg-open index.html   (Linux)
```

Or, to serve it over `http://localhost` instead of `file://` (recommended —
some browsers restrict `file://` pages more than `http://` pages):

```bash
python3 -m http.server 8000
# then open http://localhost:8000 in your browser
```

## Host it for students (GitHub Pages)

1. Push this repo to GitHub (see below).
2. On GitHub, go to **Settings → Pages**.
3. Under "Build and deployment", set **Source: Deploy from a branch**,
   branch: `main`, folder: `/ (root)`. Save.
4. GitHub gives you a public URL, typically:
   `https://<your-username>.github.io/<your-repo>/`
5. Share that link with students — that's the "easy access" link.

GitHub Pages serves the file over `https://`, so the Google Apps Script
JSONP backend will work fine there (no sandbox restrictions like the
published-Claude-artifact case).

## Push to GitHub for the first time

```bash
git init
git add index.html README.md
git commit -m "Startup Roulette app"
git branch -M main
git remote add origin https://github.com/<your-username>/<your-repo>.git
git push -u origin main
```

Then follow the "Host it for students" steps above to turn on Pages.

## Organizer panel

- Click "Organizer" (or whatever the admin link is labeled) in the app.
- PIN is set near the top of the script: `const ADMIN_PIN = "2580";` — change
  this before sharing the link publicly.
- The panel shows each student's spins, final idea, submission time, and a
  live "overdue" stopwatch for anyone past their 10-minute deadline who
  hasn't submitted yet. Auto-refreshes every 15s, or use "Refresh now".
- "Export to Excel (.csv)" downloads the full data set including timing/status.
# Social-Media-Challenge
