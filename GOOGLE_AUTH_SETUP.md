# Google Authentication Setup Guide

## 🔐 Your calendar now requires Google sign-in!

Instead of using the keypad code, users will sign in with their Google account. Each user will only see their own events.

---

## Step 1: Enable Google Authentication in Firebase

1. Go to **https://console.firebase.google.com**
2. Click on your **"dfkdb-dd7e9"** project
3. In the left sidebar, click **"Build"** → **"Authentication"**
4. Click **"Get started"** (if it's your first time)
5. Click on the **"Sign-in method"** tab
6. Find **"Google"** in the list and click on it
7. Toggle the **"Enable"** switch to ON
8. For "Public-facing name for project", use: **DFK Calendar**
9. Select a **Support email** (your email)
10. Click **"Save"**

**Done!** Google sign-in is now enabled.

---

## Step 2: Update Firestore Security Rules

Now that users authenticate, we need to secure the database so users can only access their own events.

1. In Firebase Console, go to **"Firestore Database"**
2. Click the **"Rules"** tab
3. **Copy ONLY the text below** (starting with `rules_version` and ending with the last `}`):

---

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /events/{eventId} {
      // Allow all authenticated users to read all events (shared calendar)
      allow read: if request.auth != null;
      
      // Allow authenticated users to create events with their userId
      allow create: if request.auth != null 
                    && request.auth.uid == request.resource.data.userId;
      
      // Allow authenticated users to update their own events
      allow update: if request.auth != null 
                    && request.auth.uid == resource.data.userId;
      
      // Allow authenticated users to delete their own events
      allow delete: if request.auth != null 
                    && request.auth.uid == resource.data.userId;
    }
  }
}

---

4. Paste it into the Firebase Rules editor
5. Click **"Publish"**

⚠️ **Important:** Do NOT copy the lines with dashes (---) above and below. Copy only the rules code!

**What this does:**
- ✅ Users must be signed in to access events
- ✅ Users can only see/edit/delete their own events
- ✅ New events are automatically tied to the user who created them

---

## Step 3: Test It!

1. Open `calendar.html` in your browser
2. You should see a **"Sign in with Google"** button
3. Click it and sign in with your Google account
4. After signing in, you'll see:
   - Your profile picture in the top right
   - Your name
   - A "Sign Out" button
5. Try adding an event - it will save to your account only!

---

## How It Works

### User-Specific Events
- Each event is saved with a `userId` field
- When you load the calendar, it only fetches events with your `userId`
- Other users can't see your events, and you can't see theirs

### Data Structure
```javascript
{
  id: "1735574400000",
  name: "John Doe",
  location: "Studio A", 
  time: "14:30",
  date: "2025-12-30",
  color: "#FF6B6B",
  userId: "abc123xyz456",  // ← Your unique user ID
  createdAt: Timestamp
}
```

### Sign Out
- Click the "Sign Out" button to log out
- You'll be returned to the sign-in screen
- Your events are saved in Firebase and will load when you sign back in

---

## Multiple Users

Now multiple people can use the same calendar app:

1. **User A** signs in → sees only their events
2. **User B** signs in → sees only their events
3. Events are private to each user
4. No keypad code needed!

---

## Troubleshooting

### "Sign in failed" error?
- Check that Google authentication is enabled in Firebase Console
- Make sure popup blockers aren't blocking the sign-in window

### Can't see my events after signing in?
- Check browser console (F12) for errors
- Verify Firestore security rules are published
- Make sure you're signed in with the same Google account

### Want to allow multiple users to share events?
- You'll need to modify the security rules
- Consider adding a "team" or "organization" field to events
- Contact your developer to set this up

---

## Benefits of Google Authentication

✅ **Secure** - No shared passwords or codes  
✅ **Personal** - Each user has their own calendar  
✅ **Convenient** - One-click sign in  
✅ **Professional** - Uses your Google account  
✅ **Private** - Your events stay private  

---

## What Changed?

**Before:**
- Single keypad code for everyone
- All users shared the same events
- No user authentication

**After:**
- Google sign-in required
- Each user has private events
- Secure, personalized experience

---

Your calendar is now secure and ready to use! 🎉
