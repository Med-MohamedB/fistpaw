# Building the APK from your phone only (no computer needed)

Your phone can't run Android Studio/Gradle itself in a friendly way, so the trick is:
**push the project to GitHub, and let GitHub's servers build the APK for you.** A
workflow file (`.github/workflows/build-apk.yml`) is already included in this project
that does exactly that — it installs everything and runs the same build steps
Android Studio would, in the cloud, and hands you back a downloadable `.apk`.

You still need one terminal app on your phone to get the code onto GitHub in the
first place (GitHub's website can't accept a project with a file this large through
its "upload" button reliably). That app is **Termux**.

---

## 1. Install Termux

Get it from **F-Droid**, not the Play Store (the Play Store version is outdated and
no longer maintained):

- Install F-Droid: https://f-droid.org/ (download the APK from their site, open it,
  allow install from unknown sources when prompted)
- Open F-Droid → search **Termux** → install

## 2. Set up Termux

Open Termux and run these one at a time:

```
pkg update -y && pkg upgrade -y
pkg install git unzip -y
termux-setup-storage
```

The last command will pop up an Android permission dialog — tap **Allow**, so Termux
can see your Downloads folder.

## 3. Get the project files into Termux

Download the `fist-bump-app.zip` I gave you (if you haven't already, save it from this
chat to your phone's Downloads folder). Then in Termux:

```
cd ~/storage/downloads
unzip fist-bump-app.zip -d ~/fist-bump-app
cd ~/fist-bump-app
```

(If it unzips into a nested folder, `cd` into `fist-bump-app` until `ls` shows
`package.json`, `src/`, etc.)

## 4. Create an empty GitHub repo

In your phone's browser:

1. Go to https://github.com/join if you don't have an account yet (free).
2. Once signed in, go to https://github.com/new
3. Repository name: `fist-bump-app` (anything works)
4. Leave it **Public** (Actions minutes are free for public repos) or Private — either
   is fine.
5. Do **not** check "Add a README" — leave it empty.
6. Tap **Create repository**.

## 5. Create a Personal Access Token (so Termux can push)

GitHub no longer accepts your account password for `git push` — you need a token:

1. In your browser: https://github.com/settings/tokens → **Generate new token
   (classic)**.
2. Give it any name, set expiration to whatever you're comfortable with.
3. Check the **repo** checkbox (grants push access).
4. Generate, then **copy the token** — you won't be able to see it again. Paste it
   somewhere temporarily (like your notes app) since you'll need it in a moment.

## 6. Push the code from Termux

Back in Termux (still inside `~/fist-bump-app`):

```
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/<your-username>/fist-bump-app.git
git push -u origin main
```

When it asks for a username, enter your GitHub username. When it asks for a
password, **paste the token from step 5** (not your real password).

## 7. Watch it build

1. In your browser, go to your repo: `https://github.com/<your-username>/fist-bump-app`
2. Tap the **Actions** tab. You should see a workflow run called "Build Android APK"
   already running (it starts automatically on push).
3. Tap into it and wait — a first build usually takes about 5–8 minutes.
4. When it finishes with a green checkmark, scroll to the bottom of that run page to
   the **Artifacts** section, and tap **fistpaw-debug-apk** to download it. It comes
   down as a `.zip` containing the `.apk` inside.

## 8. Install it

1. Open your phone's Files app, find the downloaded zip, and extract it (most Android
   file managers can unzip directly; if not, `unzip` it in Termux from
   `~/storage/downloads`).
2. Tap the extracted `app-debug.apk`.
3. Android will prompt you to allow installs from that source the first time — tap
   **Settings**, enable **"Allow from this source"**, go back, and install.
4. FistPaw now appears as a normal app icon on your phone.

---

## Making changes later

Anytime you edit `src/App.jsx` (in Termux with a text editor, or `nano`/`vim`), just:

```
git add .
git commit -m "update"
git push
```

Pushing to `main` automatically triggers a fresh build on GitHub — go back to the
**Actions** tab and download the new artifact when it's done.

## Notes

- This is a **debug** build, meant for installing on your own device — perfectly fine
  for personal use, just not for publishing to the Play Store.
- If the Actions build fails, tap into the failed step to see the error — the most
  common cause is a typo introduced while editing `App.jsx`.
