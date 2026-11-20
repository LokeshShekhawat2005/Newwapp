# AttendEase Android project

This package wraps the existing AttendEase web app in a native Android WebView shell. It includes Android permissions for camera-based QR scanning and a GitHub Actions workflow that builds a debug APK.

## Build the APK using GitHub Actions

This environment does not have Android SDK / Gradle installed, so it cannot compile or verify an APK locally. The included workflow builds it on GitHub's Android runner.

1. Create a GitHub repository and upload the contents of this folder.
2. Open the repository's **Actions** tab.
3. Select **Build AttendEase APK** and choose **Run workflow**.
4. When the workflow finishes, open its run and download the `AttendEase-debug-apk` artifact.
5. Extract the artifact. Install `app-debug.apk` on an Android device. If Android asks, allow installation from that source.

You can also open the project in Android Studio (Hedgehog or newer) with Android SDK 35 and JDK 17, then run the `app` configuration or use `gradle assembleDebug`.

## Current app functionality and important limits

- Student, teacher and admin demo portals are included.
- Student subject-wise attendance, attendance percentage, daily records, calendar and shortage warning are included.
- Teacher manual attendance and QR session creation are included.
- QR scanning uses the device camera permission.
- Current data storage is browser/device local storage. It does **not** sync between phones.
- Firebase initialization is only a starter hook in `app/src/main/assets/app.js`. Firebase Authentication and Firestore CRUD / live sync are not implemented yet. Do not use demo records as official attendance.

## Firebase connection

A Firebase project must be created by the app owner. No Firebase project or credentials were supplied, so this package cannot be pre-connected to a real database.

For a real shared attendance system, the next implementation needs:
1. Firebase Authentication with email/password and role assignment.
2. Firestore collections for users, subjects, attendance sessions and attendance records.
3. Firestore security rules so students can read only their own records and teachers can manage only their assigned classes.
4. Firestore transactions or unique attendance document IDs to prevent duplicate attendance.
5. Server-validated QR session expiry. A QR code generated only in local storage is not secure for official attendance.

Never publish Firebase service-account private keys in the Android app. Client Firebase web configuration is not a private key, but database access must be protected by Authentication and Firestore Security Rules.
