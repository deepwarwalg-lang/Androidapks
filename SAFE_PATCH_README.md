# AndroidForge — safe build patch

This patch adds a GitHub Actions workflow to build a **debug APK** and hardens Firestore defaults.

## What changed
- Added `.github/workflows/build-android.yml`; it uses Java 17 and Gradle 9.3.1, then uploads `app-debug.apk` as a workflow artifact.
- Firestore client rules now deny all access by default. Add Firebase Authentication and narrowly scoped user/role rules before using Firestore features.
- Automatic forwarding of verification-token records from the Cloud Function has been disabled.
- The Android app's auto-forward preference defaults to off.

## Important limitations
- This is a source patch, **not a compiled APK**. It has not been built or tested here because Android SDK/Gradle are not installed in this environment.
- The original app contains additional direct Telegram notification code and verification-record screens. Do not use it to collect or forward OTPs, SIM codes, passwords, or authentication secrets. A full safe Telegram Bot Manager should expose bot status and basic metadata only.
- Firebase Android config includes a public API key; restrict it in Google Cloud/Firebase settings to the required APIs and Android app identity. Never place a Telegram bot token in source code or a public APK.
- Firestore features will fail until proper authentication and least-privilege rules are implemented.

## Build
Run the GitHub Actions workflow named **Build Android APK** from the Actions tab. On success, download the `app-debug` artifact. A release APK requires separate signing configuration; never upload signing keys or passwords to the repository.
