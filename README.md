# The Case File — Storyline Clue Hunt

A single-page web app for a code-to-clue storyline hunt. Teams find codes
printed around a venue, type or scan them, and unlock the next part of the
story. Built for multiple teams to play at once, with a live admin panel.

## Features
- Team join/resume by name, with progress saved centrally
- Code entry with QR camera scanning (via jsQR) as an alternative to typing
- Per-clue name, location, photo, and text
- "Your clues" recap for each team
- Admin panel: manage clues, generate printable QR codes, live leaderboard,
  reset progress between events

## One-time setup: connect a free Firebase backend

The app needs somewhere to store team progress and clue data that works no
matter where you host it. Firebase's free tier (Firestore) handles this with
no server of your own required.

1. Go to https://console.firebase.google.com and sign in with a Google account
2. Click **Add project**, give it any name, finish the wizard (Google
   Analytics is optional, you can skip it)
3. In the left sidebar, go to **Build → Firestore Database → Create database**
   — choose **Start in test mode** for now (you'll tighten rules below)
4. Back in the project overview, click the **</>** (web) icon to register a
   new web app — give it any nickname, you don't need Firebase Hosting
5. Firebase will show you a `firebaseConfig` object like:
   ```js
   const firebaseConfig = {
     apiKey: "AIza...",
     authDomain: "your-project.firebaseapp.com",
     projectId: "your-project-id",
     storageBucket: "your-project.appspot.com",
     messagingSenderId: "...",
     appId: "..."
   };
   ```
6. Open `index.html` in this repo, find the `firebaseConfig` placeholder near
   the top of the `<script type="module">` tag, and paste your real values in
7. In Firebase Console, go to **Firestore Database → Rules** and set:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /{document=**} {
         allow read, write: if true;
       }
     }
   }
   ```
   This keeps things simple for a low-stakes student event — anyone with the
   link can read/write data, same as anyone with the game codes could already
   affect the hunt. Don't reuse this Firebase project for anything sensitive.

That's it — no other code changes needed. It'll show a "Connect a backend
first" screen until the config is filled in correctly.

## Hosting with a personalized link (GitHub Pages)

1. Push this repo to GitHub (create a new repo on github.com, then upload
   `index.html` and this `README.md`, or use `git push` from your machine)
2. In the repo, go to **Settings → Pages**
3. Under "Build and deployment", set Source to **Deploy from a branch**,
   branch `main`, folder `/ (root)`
4. Your site goes live at `https://<your-username>.github.io/<repo-name>/`
   — that's your personalized, shareable link
5. (Optional) If you own a custom domain, GitHub Pages supports adding it
   under the same Settings → Pages screen

## Running it locally
You can also just open `index.html` directly in a browser — no build step or
local server required, as long as your Firebase config is filled in.
