# Building FistPaw as an Android APK (Windows)

This project is now wired up with [Capacitor](https://capacitorjs.com/), which wraps the
React web app in a native Android project you can build into a real, installable `.apk`.

You only need to do the setup steps (1–2) once. After that, rebuilding is just step 5.

---

## 1. Install the one-time tools

You already have Node.js from the web-app setup. You additionally need:

- **Android Studio** — download from https://developer.android.com/studio and run the
  Windows installer. Accept the defaults. This installs the Android SDK, platform tools,
  and an emulator, which is what actually turns the app into an `.apk`.
- **Java (JDK 17)** — Android Studio bundles its own JDK, so you normally don't need to
  install this separately. If a build step later complains it can't find Java, install
  [Temurin JDK 17](https://adoptium.net/) and restart your terminal.

Open Android Studio once after installing so it can finish downloading SDK components
(the "SDK Manager" screen). Leave everything at its default selections and let it finish.

## 2. Set ANDROID_HOME (one time)

Capacitor's command line needs to know where the Android SDK lives.

1. Open Android Studio → **More Actions → SDK Manager**, and note the "Android SDK
   Location" path near the top (usually `C:\Users\<you>\AppData\Local\Android\Sdk`).
2. Open the Start Menu → search **"Environment Variables"** → **Edit the system
   environment variables** → **Environment Variables** button.
3. Under "User variables", click **New**:
   - Variable name: `ANDROID_HOME`
   - Variable value: the SDK path from step 1
4. Also edit your `Path` variable and add these two entries:
   - `%ANDROID_HOME%\platform-tools`
   - `%ANDROID_HOME%\tools`
5. Close and reopen Command Prompt so the changes take effect.

## 3. Install project dependencies

In Command Prompt, inside the unzipped `fist-bump-app` folder:

```
npm install
```

This pulls in Capacitor along with the existing React/Vite dependencies.

## 4. Generate the native Android project

This only needs to be run once — it creates an `android/` folder containing a full
Android Studio project:

```
npm run build
npx cap add android
npx cap sync android
```

- `npm run build` compiles the React app into static files (`dist/`).
- `npx cap add android` scaffolds the native Android project and points it at `dist/`.
- `npx cap sync android` copies your latest web build into that native project.

## 5. Build the APK

You have two options:

### Option A — Android Studio (easiest first time)

```
npx cap open android
```

This launches Android Studio with the project open. Once it finishes indexing/Gradle
syncing (first time can take a few minutes):

- Menu bar → **Build → Build Bundle(s) / APK(s) → Build APK(s)**.
- When it finishes, click the **"locate"** link in the notification, or find it at:
  ```
  android\app\build\outputs\apk\debug\app-debug.apk
  ```

### Option B — Command line only

From inside the `fist-bump-app` folder:

```
npm run android:build
```

(This runs the web build, syncs it, then invokes Gradle directly.) The resulting APK
lands in the same place:
```
android\app\build\outputs\apk\debug\app-debug.apk
```

## 6. Install the APK on your phone

1. Copy `app-debug.apk` to your phone (USB cable, email it to yourself, Google Drive,
   whatever's easiest).
2. On your phone, tap the file. Android will ask you to allow installs from that source
   the first time — tap **Settings**, enable **"Allow from this source"**, then go back
   and install.
3. FistPaw now appears as a normal app icon and runs full-screen — the streak data is
   stored on-device (in the app's local storage), so it persists across launches.

## Making changes later

Whenever you edit `src/App.jsx` and want a fresh APK:

```
npm run android:build
```

(or `npm run build && npx cap sync android` followed by rebuilding in Android Studio).
Capacitor keeps the native shell in `android/` and just swaps in your latest web build.

## Notes

- This produces a **debug** APK, which is fine for side-loading on your own device.
  If you ever want to publish to the Play Store, you'd need to generate a signed
  **release** build — a different process (Android Studio's Build → Generate Signed
  Bundle/APK) that Google requires for store distribution, not needed for side-loading.
- The app has no network/backend dependency, so once installed it works fully offline.
