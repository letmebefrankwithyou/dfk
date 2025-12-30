# Firebase Setup Guide

Follow these steps to enable cloud sync for your calendar events:

## Step 1: Create a Firebase Project

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click **"Add project"** or sign in with your Google account
3. Enter a project name (e.g., "calendar-app")
4. Click **"Create project"** and wait for it to initialize
5. Click **"Continue"**

## Step 2: Create a Firestore Database

1. In the Firebase Console, click **"Build"** → **"Firestore Database"**
2. Click **"Create database"**
3. Select **"Start in test mode"** (for development)
   - This allows read/write without authentication for now
   - You can add authentication later if needed
4. Choose a location closest to you (e.g., "us-central1")
5. Click **"Create"**

## Step 3: Get Your Firebase Config

1. In the Firebase Console, click the **gear icon** ⚙️ (top left)
2. Select **"Project settings"**
3. Scroll down to **"Your apps"** section
4. Click **"Web"** (if no apps, click **"Add app"**)
5. Copy the entire config object that looks like:

```javascript
{
  apiKey: "YOUR_API_KEY",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-project-id",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123def456"
}
```

## Step 4: Update Your Calendar

1. Open `calendar.html`
2. Find the comment `// FIREBASE CONFIG - REPLACE WITH YOUR CONFIG`
3. Replace the entire config object with your Firebase credentials from Step 3
4. Save the file

## Done! 

Your calendar will now:
- ✅ Save events to Firebase Cloud
- ✅ Load events from any device
- ✅ Sync in real-time across devices
- ✅ Keep a backup of all your data

## Security Note

Currently, the database is in **test mode** (public read/write). For production, you should:
1. Set up Firebase Authentication (Google Sign-in)
2. Create security rules to restrict access to your events only

Let me know if you need help with those steps!
