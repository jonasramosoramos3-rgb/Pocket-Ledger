<p align="center">
  <img src=".github/social-preview.png" alt="Pocket Ledger — a tiny offline-first budget tracker" width="100%">
</p>

<h1 align="center">Pocket Ledger</h1>

<p align="center">
  A tiny, offline-first budget tracker in a single HTML file.<br>
  No accounts, no sign-ups, no servers, no tracking. Your money data stays on your device.
</p>

<p align="center">
  <img alt="license" src="https://img.shields.io/badge/license-MIT-10b981">
  <img alt="no dependencies" src="https://img.shields.io/badge/dependencies-0-10b981">
  <img alt="no build step" src="https://img.shields.io/badge/build%20step-none-10b981">
  <img alt="offline" src="https://img.shields.io/badge/works-offline-10b981">
</p>

<p align="center">
  <img src=".github/screenshots/overview-dark.png" alt="Overview" width="200">
  <img src=".github/screenshots/activity-dark.png" alt="Activity list" width="200">
  <img src=".github/screenshots/budgets-dark.png" alt="Category budgets" width="200">
  <img src=".github/screenshots/add-transaction.png" alt="Add a transaction" width="200">
  <img src=".github/screenshots/overview-light.png" alt="Light theme" width="200">
</p>

---

## What it does

- **Track money in and out** — log an expense or income in about ten seconds. Amount, category, date, optional note.
- **See the month at a glance** — balance, total in, total out, savings rate, and a category donut chart.
- **Set category budgets** — type a ceiling per category and it saves itself. Each row shows spent / remaining, and turns red when you go over.
- **Look back** — a 6-month income vs. expense chart, plus your full history grouped by day.
- **Filter and search** — by type, by month or all time, by note or category.
- **Two themes** — dark by default, light mode if you prefer. It follows your system on first run.
- **10 currencies** — defaults to ₱ PHP; switch to $, €, £, ¥, ₹ and more in Settings.
- **Backups you own** — export JSON (restorable) or CSV (opens in Excel/Sheets), import JSON to move devices.
- **Installable PWA** — add it to your home screen on a phone and it works with no connection.
- **Android home-screen widget** — this month's balance plus a one-tap quick-add button (APK only).

## Quick start

| How | Steps |
| --- | --- |
| **Just use it** | Download `index.html` and double-click it. That is the whole app. |
| **Clone it** | `git clone <your-repo-url> && cd budget-tracker`, then open `index.html`. |
| **Host it** | Push to GitHub, then **Settings → Pages → Source: GitHub Actions**. The included workflow deploys on every push. |
| **Local server** | `npx serve .` or `python3 -m http.server 8080`, then open `http://localhost:8080` |

> **Note:** opening `index.html` straight from disk works fully. The only features that need HTTPS are the "install app" prompt and offline caching — both switch on automatically once it is on GitHub Pages or any HTTPS host.

## Get it as an Android app

There is a real, installable **APK** — same app, wrapped in a native shell. No
Play Store, no account, no build tools needed to use it.

1. Copy `Pocket-Ledger-v1.0.apk` to your phone.
2. Tap it and allow "install from unknown sources" when Android asks.
3. Open **Pocket Ledger** from your app drawer.

Works on **Android 5.1+**, ~3.8 MB installed, and runs fully offline because the
assets ship inside the APK.

What the native shell adds on top of the web version:

- Launcher icon (adaptive + round, all densities) and a branded splash screen
- **Hardware back button** — closes the entry sheet, then the dialog, then backs out to Overview before exiting
- Status bar recolours to match dark/light theme
- **Backup export opens the Android share sheet** (Drive, Gmail, Files) instead of a silent download that WebViews ignore
- No browser chrome, no URL bar — it just looks like an app
- **Home-screen widget** — shows this month's balance and has a **+** button that opens the entry sheet directly (long-press your home screen → Widgets → Pocket Ledger)

The rebuildable Capacitor project plus full instructions live in
`pocket-ledger-android-project.zip` (`README-ANDROID.md` inside).

### Prefer no APK?

The same app installs from the browser once it is on HTTPS (GitHub Pages):
Chrome menu → **Install app**. You get the icon and offline mode; you just miss
the hardware back-button handling and the native share sheet.

## Your data

Everything lives in your browser's `localStorage` under the key `pocketledger.v1`. Nothing is ever uploaded, which also means:

- Clearing browser data deletes your ledger — **export a JSON backup once in a while** (Settings → Export JSON).
- Data does not sync between devices or browsers. Use export → import to move it.
- Private/incognito windows let you in, then throw the data away when you close the tab.

There is no account, no cloud, and no analytics call anywhere in the code.

## Project structure

```
budget-tracker/
├── index.html                     # the whole app: markup, styles, logic
├── manifest.webmanifest           # PWA metadata
├── sw.js                          # offline cache (service worker)
├── icons/                         # app icons (SVG + PNG, incl. maskable)
├── .github/
│   ├── social-preview.png         # repo social preview
│   ├── screenshots/               # README images
│   └── workflows/pages.yml        # auto-deploy to GitHub Pages
├── README.md
└── LICENSE
```

Shipped alongside this folder:

```
Pocket-Ledger-v1.0.apk             # ready-to-install Android app
pocket-ledger-android-project.zip  # Capacitor source to rebuild the APK
```

No build step, no `node_modules`, no framework, no bundler. Edit `index.html`, refresh, done.

## Making it yours

Everything worth changing sits in a few clearly labelled blocks near the top of the `<script>`:

```js
// Change the default currency
var CURRENCIES = [{ code: 'PHP', symbol: '₱', locale: 'en-PH', name: 'Philippine Peso' }, /* ... */];

// Add, rename or recolour categories (icons are keys in the ICONS map next door)
var EXPENSE_CATS = [{ id: 'food', name: 'Food', long: 'Food & Groceries', icon: 'food', color: '#f97316' }, /* ... */];
```

Theme colours are CSS variables at the top of the `<style>` block — change `--accent` and the entire app follows. Category glyphs are inline SVGs, so nothing depends on an emoji font being installed.

Two small shims let the exact same file run inside the Android APK: `isNative()` checks for Capacitor, and `saveFile()` routes exports to the native share sheet when it is available (falling back to a normal browser download otherwise). On the web both are inert.

## Keyboard shortcuts

| Key | Action |
| --- | --- |
| `N` | New transaction |
| `Esc` | Close the sheet / cancel a dialog |

## Roadmap ideas

These fit the current architecture cleanly if you want to keep hacking:

- [ ] Recurring transactions (rent, salary) generated when a month opens
- [ ] Budget rollover for unspent categories
- [ ] Per-category trends across months
- [ ] Optional passcode lock
- [ ] Bank CSV import with column mapping

## Contributing

Issues and pull requests are welcome. Keep it dependency-free and single-file — that is the whole point of the project.

## License

[MIT](LICENSE) — do whatever you want with it.
