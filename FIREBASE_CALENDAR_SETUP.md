# Firebase Calendar Setup Guide

## Quick Start (Do This First!)

**You don't have a Firestore database yet. Here's what to do:**

1. Go to **https://console.firebase.google.com**
2. Click on your **"dfkdb-dd7e9"** project
3. Click **"Firestore Database"** in the left menu (under Build)
4. Click **"Create database"** button
5. Select **"Start in test mode"** → Click **Next**
6. Choose **"us-east1"** (or closest region) → Click **Enable**
7. Wait 1-2 minutes for setup to complete

**That's it!** Your calendar will now save to the cloud.

---

## Overview
Your calendar needs to store instructor, date, time, and location information. Firebase Firestore will handle this automatically.

---

## Step 1: Enable Firestore Database

1. Go to **[firebase.google.com/console](https://console.firebase.google.com)**
2. Sign in with your Google account
3. Select your **"dfkdb-dd7e9"** project
4. In the left sidebar, click **"Build"** → **"Firestore Database"**
5. If you see "Get started" or "Create database", click it
6. Choose **"Start in test mode"** (for now - we'll secure it later)
7. Select a Cloud Firestore location (choose closest to you, like `us-east1`)
8. Click **"Enable"**

**Wait 1-2 minutes for Firestore to provision.**

---

## Step 2: Set Up Security Rules (IMPORTANT!)

Once Firestore is enabled:

1. In Firestore Database, click the **"Rules"** tab
2. Replace the rules with this:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Allow anyone to read/write events for now (development only)
    match /events/{eventId} {
      allow read, write: if true;
    }
  }
}
```

3. Click **"Publish"**

⚠️ **Note**: These rules allow anyone to access your data. For production, you should add authentication.

---

## Step 3: Verify Your Configuration

Your `calendar.html` already has the correct Firebase config:

```javascript
const firebaseConfig = {
    apiKey: "AIzaSyCuj8UOStRvsZPnrUpxyXtFpxbSeSLA2ok",
    authDomain: "dfkdb-dd7e9.firebaseapp.com",
    projectId: "dfkdb-dd7e9",
    storageBucket: "dfkdb-dd7e9.firebasestorage.app",
    messagingSenderId: "632770320558",
    appId: "1:632770320558:web:ac9d091d26fc97b1dfb53f",
    measurementId: "G-6YH8G2V4R7"
};
```

✅ **No code changes needed!**

---

## Step 5: Security Rules (Optional but Recommended)

To prevent anyone from deleting your events, set up security rules:

1. In Firestore, click **"Rules"** tab
2. Replace the code with:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /events/{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

3. Click **"Publish"**

This requires users to sign in (optional - skip if you want it public)

---

## Step 6: Test It Works

1. Open your code pad (`index.html`)
2. Enter code **3354**
3. Click a day in the calendar
4. Fill in:
   - **Event Name** = Instructor name (e.g., "John Smith")
   - **Location** = Location (e.g., "Room 101")
   - **Time** = Time (e.g., "2:30 PM")
5. Click **"Save Event"**
6. Go to Firebase Console → Firestore Database
7. You should see your event appear in the `events` collection!

---

## Step 7: See Your Data in Firebase

1. Open **[Firebase Console](https://console.firebase.google.com)**
2. Click your project
3. Go to **Build** → **Firestore Database**
4. Look for the **events** collection
5. Click it to expand and see all saved events with:
   - Instructor name
   - Location
   - Time
   - Date

---

## How It Works

✅ **When you add an event:**
- Calendar saves to Firestore (cloud database)
- Data syncs instantly

✅ **When you load the calendar:**
- App reads from Firestore
- All your events load automatically
- Works on any device

✅ **All devices see the same data:**
- Phone, tablet, computer all connect to same database
- Changes appear everywhere instantly

---

## Troubleshooting

**Events not saving?**
- Check browser console (F12 → Console tab)
- Make sure Firebase credentials are correct in calendar.html
- Check your internet connection

**Events not showing?**
- Refresh the page
- Check Firestore in Firebase Console to confirm data exists
- Clear browser cache (Ctrl+Shift+Delete)

**Permission denied error?**
- Your Firestore is in "test mode" - this is fine
- Test mode allows read/write without authentication
- Later you can add authentication if you want security

---

## That's It! 🎉

Your calendar is now fully connected to Firebase. All instructor, date, time, and location data will be stored in the cloud and accessible from any device!
