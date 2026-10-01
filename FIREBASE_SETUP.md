# Turning on shared rankings (Firebase)

Until this is set up, Hunch saves rankings only in the creator's own browser. With Firebase, every ranking a signed-in user publishes shows up for everyone, live.

## 1. Create the project
1. Go to https://console.firebase.google.com and click **Add project** (name it e.g. `hunch`). Google Analytics can be off.
2. In **Build → Authentication → Get started → Sign-in method**, enable **Google**.
3. In **Authentication → Settings → Authorized domains**, add `shekharsanskar9.github.io`. `localhost` is already allowed for testing.
4. In **Build → Firestore Database → Create database**. Choose production mode and any region.
5. In Firestore, open the **Rules** tab, paste the contents of [`firestore.rules`](firestore.rules), and **Publish**.

## 2. Get the config
1. **Project settings (gear icon) → General → Your apps → Web (`</>`)**, register an app named `hunch`.
2. Copy the `firebaseConfig` values.

## 3. Paste it into the app
Open `index.html`, find `FIREBASE_CONFIG`, and fill it in:

```js
const FIREBASE_CONFIG={apiKey:'...',authDomain:'...',projectId:'...',appId:'...'};
```

These values are public identifiers, not secrets. Access is controlled by the Firestore rules above, not by hiding the config.

## What users get
- **Sign in with Google** appears in the top bar.
- **Set a ranking** requires sign-in. Published rankings appear for everyone immediately, labelled with the creator's name.
- Creators can delete their own rankings. Nobody can edit or delete anyone else's.
- Local rankings made before this was enabled can be pushed online with **Publish**.
