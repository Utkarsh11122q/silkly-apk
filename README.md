# Silkly — build the APK

This folder builds the Silkly Android app into an installable **`.apk`** you can send
straight to people — no Play Store, no $25 fee. Pick one of the two paths below.

> A "debug" APK is produced. It installs and runs exactly like a normal app and is fine
> for handing to people directly. It's auto-signed, so you don't manage a keystore. (A
> release-signed build is only needed for the Play Store — ask me and I'll add it.)

---

## ⚠️ Do this first (one minute)

Open `www/index.html` and put in your Supabase values near the top of the script:

```js
var SUPABASE_URL  = "https://YOUR-PROJECT.supabase.co";
var SUPABASE_ANON = "YOUR-ANON-PUBLIC-KEY";
```

The APK builds fine without this, but nobody can sign in until it's filled in (the app
needs the backend from the main project's `SETUP.md`). Do the backend setup first.

---

## Path A — build in the cloud (no tools to install) ✅ recommended

GitHub compiles it for you on its servers and gives you the APK to download.

1. Create a free account at **github.com** if you don't have one.
2. Make a **new repository** (private is fine). Upload everything in this folder to it
   (drag-and-drop in the browser works, or use git). Make sure the `.github` folder is
   included.
3. Open the repo's **Actions** tab. The "Build APK" job starts automatically on upload —
   or click **Run workflow** to start it.
4. When it finishes (3–5 min, green check), open the run and download the
   **`silkly-apk`** artifact at the bottom. Inside is `app-debug.apk`.
5. Send that file to anyone. They open it on Android, allow install from this source once,
   and it's installed.

To ship an update later: change `www/index.html`, push it again, download the new APK.

---

## Path B — build on a computer (one command)

Needs **Node.js** and a **Java 17 JDK** installed. The Android SDK is fetched
automatically by Gradle on first run.

```bash
npm install
npx cap add android
npx cap sync android
cd android && ./gradlew assembleDebug
```

The APK lands at:

```
android/app/build/outputs/apk/debug/app-debug.apk
```

(If you have Claude Cowork or a dev machine, this is the path it can run for you.)

---

## What's in here

```
silkly-apk/
├─ www/index.html                  ← the app (fill in your Supabase keys here)
├─ capacitor.config.json           ← app id (com.silkly.app) + name (Silkly)
├─ package.json                    ← Capacitor dependencies
├─ .github/workflows/build-apk.yml ← the cloud build (Path A)
└─ .gitignore
```

To change the app's name or id, edit `capacitor.config.json` before building. Want a
custom app icon and splash image? Send me a square logo and I'll wire those in.
