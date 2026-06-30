# Waypoint Run — Android app

This is a small Android wrapper around the route-planner web app: it loads the
HTML/JS app (`app/src/main/assets/www/index.html`) inside a full-screen
WebView, and bridges browser GPS / file-upload prompts to native Android
permission dialogs so live location and CSV/JSON upload work like a normal app.

No Android Studio or local SDK is required — GitHub builds the APK for you.

## Build the APK on GitHub (no local setup)

1. Create a new **empty** GitHub repository.
2. Upload every file/folder in this project to that repo, preserving the
   folder structure (the `.github/workflows/build.yml` file must end up at
   that exact path in the repo root).
   - Easiest way: on github.com, use **Add file → Upload files**, drag the
     whole folder in, and commit.
3. Go to the **Actions** tab of your repo. A workflow called **"Build APK"**
   will run automatically (or click **Run workflow** if it doesn't start).
4. When it finishes (a few minutes), open the completed run and download the
   **`waypoint-run-debug-apk`** artifact — that's a zip containing
   `app-debug.apk`.
5. Transfer `app-debug.apk` to your Android phone (email it to yourself,
   Google Drive, USB, etc.) and tap it to install.
   - Android will warn about installing from an unknown source the first
     time — that's expected for a non-Play-Store app. Allow it for this
     file/app.

## What's in the project

```
WaypointRun/
├── build.gradle, settings.gradle, gradle.properties   — Gradle project config
├── app/
│   ├── build.gradle                                   — app module config
│   └── src/main/
│       ├── AndroidManifest.xml                        — permissions, launcher activity
│       ├── java/com/waypointrun/app/MainActivity.kt    — WebView host + GPS/file bridges
│       ├── res/drawable/ic_launcher.xml                — app icon (vector, no binary assets)
│       └── assets/www/index.html                       — the route planner app itself
└── .github/workflows/build.yml                         — CI workflow that builds the APK
```

## Updating the app later

To change anything about the route planner's behavior or design, just edit
`app/src/main/assets/www/index.html` and push the change — GitHub Actions
will rebuild the APK automatically on every push to `main`/`master`, or you
can trigger it manually from the Actions tab.

## Notes

- This builds a **debug** APK, which is self-signed automatically by Gradle
  and fine for installing on your own device. It is not signed for Google
  Play distribution — if you eventually want to publish to the Play Store,
  you'd need to set up a release signing key, which isn't included here.
- The app needs internet access to load map tiles (OpenStreetMap) and
  location permission to show your live position — both are already
  declared in the manifest and requested at runtime.
- Minimum supported Android version: Android 5.0 (API 21).
