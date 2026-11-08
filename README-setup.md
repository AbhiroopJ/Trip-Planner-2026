# Family Trip Ledger: setup

`index.html` is the whole app. It needs a free Firebase project to store the shared data
(expenses, settlements, trip settings, banner photo). About 5 minutes.

## 1. Create the Firebase project
1. Go to https://console.firebase.google.com and click **Add project** (no Analytics needed).
2. **Build → Firestore Database → Create database**. Pick a region near you (e.g. `asia-south1`)
   and start in **production mode**.
3. **Project settings (gear) → Your apps → Web (`</>`)**. Register an app (skip Hosting).
   Copy the `firebaseConfig` values.

## 2. Paste the config
Open `index.html` and fill in the `window.FIREBASE_CONFIG` block at the top.

## 3. Turn on the family passcode
The passcode is the password of one shared Firebase login that everyone uses.

1. Firebase console → **Build → Authentication → Get started**.
2. **Sign-in method** tab → **Email/Password** → switch **Enable** on → **Save**.
3. **Users** tab → **Add user**. Email: `family@tripledger.app` (must match `window.FAMILY_EMAIL` in `index.html`).
   Password: your family passcode (at least 6 characters). Click **Add user**.
4. To change the passcode later: Users tab → the user's `⋮` menu → **Reset password**, or delete the user and add it again.

## 4. Set the Firestore rules
Firestore → **Rules** tab, replace everything with this and **Publish**:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /expenses/{id}     { allow read, write: if request.auth != null; }
    match /settlements/{id}  { allow read, write: if request.auth != null; }
    match /settings/{id}     { allow read, write: if request.auth != null; }
  }
}
```

Only someone who has entered the passcode can read or change the ledger, even if they find the page link or
call the database directly. Each device stays unlocked until someone taps **Lock** or clears the browser data.

## 5. Deploy to GitHub Pages
1. Create a GitHub repo and add `index.html` at the root.
2. Repo **Settings → Pages → Deploy from a branch → `main` / root**.
3. Your link will be `https://<your-username>.github.io/<repo-name>/`. Send it to the family; it opens in any
   browser or in-app browser, on phone or laptop.

Notes: `Family Trip Ledger.html` is the older version that only works inside Claude. Ignore it for deployment.
