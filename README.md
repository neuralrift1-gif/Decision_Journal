# Decision Ledger

A personal decision journal — log the context, the options you weighed, and
the call you made, then come back later and record what actually happened.
Free to host, syncs across your phone/tablet/laptop via your own free
Firebase project, and lives entirely in this repo — no subscriptions,
nothing to pay for.

This is a static site: plain HTML/CSS/JS, no build step, no `npm install`.
It's hosted on GitHub Pages and stores your data in Firestore (Firebase's
free-tier database), guarded so only your signed-in Google account can ever
read or write it.

## One-time setup (about 10 minutes)

### 1. Create a free Firebase project

1. Go to [console.firebase.google.com](https://console.firebase.google.com), sign in, click **Add project**. Name it anything (e.g. "decision-ledger"). You can skip Google Analytics.
2. In the left sidebar: **Build → Firestore Database → Create database**. Choose **Start in production mode**, pick any location near you.
3. In the left sidebar: **Build → Authentication → Get started**. Under **Sign-in method**, enable **Google**. It'll ask for a support email — use your own.
4. Click the gear icon next to "Project Overview" → **Project settings** → scroll to **Your apps** → click the `</>` (web) icon → give it any nickname → register.
5. Firebase shows you a `firebaseConfig` object. Copy it.

### 2. Fill in your config

Open `firebase-config.js` in this repo and replace the placeholder values
with the ones Firebase just gave you. It should look like:

```js
export const firebaseConfig = {
  apiKey: "AIza...",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-project",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "...",
  appId: "..."
};
```

These values are project locators, not secrets — Firebase expects them in
public client code. Your data is protected by the security rules below, not
by hiding this file.

### 3. Apply the security rules

In the Firebase console: **Build → Firestore Database → Rules** tab. Delete
what's there and paste the contents of `firestore.rules` from this repo,
then click **Publish**. This restricts every document to
`users/<your-uid>/...` so nobody but you can ever read or write your
entries.

### 4. Push this repo to GitHub

```bash
git init
git add .
git commit -m "Decision Ledger"
git branch -M main
git remote add origin https://github.com/neuralrift1-gif/Decision_Journal.git
git push -u origin main
```

If you edit `firebase-config.js` after this initial push, commit and push
that change too:

```bash
git add firebase-config.js
git commit -m "Add Firebase config"
git push
```

### 5. Turn on GitHub Pages

On GitHub: your repo → **Settings → Pages** → under "Build and deployment",
set **Source** to "Deploy from a branch", branch `main`, folder `/ (root)`
→ **Save**. GitHub gives you a URL like:

```
https://neuralrift1-gif.github.io/Decision_Journal/
```

That's your journal's permanent address — open it on your phone, tablet, or
laptop, sign in with Google, and everything you log syncs automatically
across all of them.

### 6. Authorize the domain (one more Firebase step)

Back in Firebase console → **Authentication → Settings → Authorized
domains** → **Add domain** → add `neuralrift1-gif.github.io`. Without this,
Google sign-in will refuse to work on your live site (it works fine on
`localhost` during testing, which is why this step is easy to miss).

## Using it day to day

Same as the version you already had — log a decision (context, options
considered, what you decided, why, and what "worked" would look like), set
a date to revisit it, and when that date arrives the entry shows as "Due
for review." Open it, fill in what actually happened and the lesson for
next time, and it closes the loop. Use "Manage areas & categories" to add
new projects as they come up — nothing is hardcoded.

## Files

- `index.html` — the whole app (markup, styles, and logic in one file).
- `firebase-config.js` — your project's connection details (edit this).
- `firestore.rules` — the security rules to paste into the Firebase console.

## Costs

Firebase's free "Spark" plan covers this comfortably — a personal decision
journal is a tiny fraction of its free daily quota. GitHub Pages is free
for public and for most private repos. No credit card is required for
either at this scale.
