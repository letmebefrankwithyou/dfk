# Firebase Calendar Setup Guide

## Overview
Your calendar needs to store instructor, date, time, and location information. Firebase Firestore will handle this automatically.

---

## Step 1: Access Firebase Console

1. Go to **[firebase.google.com](https://firebase.google.com)**
2. Click **"Go to console"** (top right)
3. Sign in with your Google account
4. Select your **"dfkdb-dd7e9"** project

---

## Step 2: Check Your Firestore Database

1. In the left sidebar, click **"Build"** → **"Firestore Database"**
2. You should see your database is already created
3. If you see a message about "test mode", that's fine for now

---

## Step 3: Understand the Data Structure

Your calendar app currently saves events like this:

```
Collection: "events"
├── Document ID: (auto-generated)
│   ├── name: "Meeting" (INSTRUCTOR)
│   ├── location: "New York" (LOCATION)
│   ├── time: "14:30" (TIME - 24hr format)
│   ├── date: "2025-12-29" (DATE - YYYY-MM-DD)
│   ├── color: "#FF6B6B"
│   └── createdAt: (timestamp)
```

---

## Step 4: Update Calendar.html

Your `calendar.html` already has the correct Firebase setup! It saves:
- ✅ Event name (INSTRUCTOR)
- ✅ Location (LOCATION)
- ✅ Time (TIME)
- ✅ Date (DATE)

**No changes needed to the code!**

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
