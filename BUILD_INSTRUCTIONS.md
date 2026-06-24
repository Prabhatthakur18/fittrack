# FitTrack — Build APK Instructions

## What you need on your PC
- Node.js 18+ (already have it)
- Android Studio (download from https://developer.android.com/studio)
- JDK 17 (Android Studio installs this automatically)

---

## Step 1 — Extract this ZIP on your PC

Extract `fittrack-capacitor.zip` to a folder, e.g. `C:\Projects\fittrack-capacitor`

---

## Step 2 — Install dependencies

Open terminal / command prompt in the extracted folder:

```bash
npm install
```

---

## Step 3 — Add Android platform

```bash
npx cap add android
```

This creates an `android/` folder — a full Android Studio project.

---

## Step 4 — Sync web files into Android project

```bash
npx cap sync android
```

Run this every time you change `www/index.html`.

---

## Step 5 — Add the ACTIVITY_RECOGNITION permission

Open `android/app/src/main/AndroidManifest.xml` in any text editor.

Add these lines INSIDE the `<manifest>` tag, before `<application>`:

```xml
<uses-permission android:name="android.permission.ACTIVITY_RECOGNITION"/>
<uses-permission android:name="com.google.android.gms.permission.ACTIVITY_RECOGNITION"/>
```

---

## Step 6 — Register the Pedometer plugin

Open `android/app/src/main/java/com/fittrack/app/MainActivity.java`

Replace the entire file with:

```java
package com.fittrack.app;

import android.os.Bundle;
import com.getcapacitor.BridgeActivity;
import io.capgo.capgopedomter.CapacitorPedometer;

public class MainActivity extends BridgeActivity {
    @Override
    public void onCreate(Bundle savedInstanceState) {
        registerPlugin(CapacitorPedometer.class);
        super.onCreate(savedInstanceState);
    }
}
```

---

## Step 7 — Open in Android Studio

```bash
npx cap open android
```

Android Studio opens automatically with the project loaded.

---

## Step 8 — Build the APK

In Android Studio:
1. Wait for Gradle sync to finish (bottom bar shows progress)
2. Go to **Build → Build Bundle(s) / APK(s) → Build APK(s)**
3. Wait ~2 minutes
4. Click **"locate"** in the notification that appears
5. Your APK is at: `android/app/build/outputs/apk/debug/app-debug.apk`

---

## Step 9 — Install on your phone

Transfer `app-debug.apk` to your Android phone via:
- Google Drive → upload → open on phone → Install
- WhatsApp to yourself → download → Install
- USB cable → copy to Downloads folder

On phone:
- Tap the APK → if blocked → Settings → Allow from this source → Install
- Grant **Physical Activity** permission when the app asks

---

## How steps work in the APK

- Uses Android's `TYPE_STEP_COUNTER` hardware sensor
- Counts automatically in background even when app is minimized
- Resets to 0 at midnight automatically
- Open the app anytime to see current count
- Pause/Resume button if you want to stop counting temporarily

---

## Troubleshooting

**Gradle sync fails** → In Android Studio: File → Invalidate Caches → Restart

**"Package not found" error** → Run `npm install` again then `npx cap sync android`

**Steps always show 0** → Make sure you granted Physical Activity permission on first launch
