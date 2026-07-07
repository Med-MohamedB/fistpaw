# FistPaw

A fist-bump streak/habit tracker app (React + Vite), themed around the "Fist Bump" cat memes — tap to bump, build a streak, unlock achievements, freeze badges to protect your streak, avatars, sound/haptics, and stats.

The 7 day-of-week cat photos are already embedded inside `src/App.jsx` (as base64 data), so there are no separate image files to manage — the whole app is two source files.

## Requirements

- **Node.js 18 or newer** (includes npm). Download from https://nodejs.org — grab the "LTS" installer for Windows and run it (accept the defaults).

To check you already have it, open **Command Prompt** or **PowerShell** and run:

```
node -v
npm -v
```

If both print version numbers, you're set. If not, install Node.js first.

## Install & run on Windows

1. Unzip this folder anywhere, e.g. `C:\Users\<you>\Desktop\fist-bump-app`.
2. Open **Command Prompt** (or PowerShell) and `cd` into the folder:
   ```
   cd C:\Users\<you>\Desktop\fist-bump-app
   ```
3. Install dependencies:
   ```
   npm install
   ```
4. Start the app:
   ```
   npm run dev
   ```
5. Vite will print a local URL (usually `http://localhost:5173`) and should open it automatically in your browser. If it doesn't, open that link manually.

That's it — the app runs entirely in your browser, no backend needed. Your streak/stats/settings are saved in the browser's `localStorage`, so they'll persist between sessions on the same browser.

## Building a production version (optional)

If you want a static, optimized build (e.g. to host it somewhere or open without the dev server):

```
npm run build
```

This creates a `dist/` folder. You can preview it locally with:

```
npm run preview
```

## Project structure

```
fist-bump-app/
├── index.html          # HTML entry point
├── package.json         # dependencies & scripts
├── vite.config.js       # Vite config
├── src/
│   ├── main.jsx          # React entry point, mounts <FistPaw />
│   └── App.jsx            # The full FistPaw app (component logic + embedded cat images)
└── public/               # (empty — images are embedded in App.jsx, no static assets needed)
```

## Troubleshooting

- **`npm install` fails / permission errors**: make sure you're not running from inside a OneDrive-synced folder with restrictive permissions, or try running Command Prompt as Administrator.
- **Port 5173 already in use**: stop whatever else is using it, or edit `server.port` in `vite.config.js`.
- **Blank page in browser**: open the browser dev tools (F12) → Console tab, and check for errors; make sure `npm install` completed without errors first.
