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

## 3. Set the Firestore rules
Firestore → **Rules** tab, paste this and **Publish**:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /expenses/{id}     { allow read, write: if true; }
    match /settlements/{id}  { allow read, write: if true; }
    match /settings/{id}     { allow read, write: if true; }
  }
}
```

This means anyone who has the page link can read and edit the ledger. There is no login. That is fine
for a family trip if you only share the link with family, but do not post the link publicly.
(The Firebase config in the page is not a secret; the rules are what protect the data.)

## 4. Deploy to GitHub Pages
1. Create a GitHub repo and add `index.html` at the root.
2. Repo **Settings → Pages → Deploy from a branch → `main` / root**.
3. Your link will be `https://<your-username>.github.io/<repo-name>/`. Send it to the family; it opens in any
   browser or in-app browser, on phone or laptop.

Notes: `Family Trip Ledger.html` is the older version that only works inside Claude. Ignore it for deployment.
