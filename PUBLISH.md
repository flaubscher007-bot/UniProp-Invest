# Kampus — publish as an installable app (PWA)

This folder is a complete Progressive Web App. Once it's on the web (over HTTPS),
it installs on **Android, iPhone and PC** like a normal app, works **offline**,
shows **listing previews**, and can be turned into a real **APK**.

**Files (keep them together, don't rename):**
`index.html` · `manifest.webmanifest` · `service-worker.js` · `icon-192.png` · `icon-512.png` · `icon-512-maskable.png`

---

## Option A — Netlify Drop (fastest, ~2 minutes, free)

1. Go to **https://app.netlify.com/drop**
2. **Drag this whole `kampus-pwa` folder** onto the page (not the files one by one — the folder).
3. Netlify gives you a link like `https://kampus-yourname.netlify.app`.
   (Free Netlify account lets you rename it and keep it permanently.)
4. **WhatsApp that link to your wife.** On each phone/PC, open it and install (steps below).

## Option B — GitHub Pages (free, permanent)

1. Create a free GitHub account → **New repository** (e.g. `kampus`), Public.
2. Upload all the files in this folder into the repo.
3. **Settings → Pages →** Source: `main` branch, `/root` → Save.
4. Your app appears at `https://YOURNAME.github.io/kampus/`. Share that link.

> Both give you **HTTPS**, which a PWA needs for install + offline. Opening the raw
> file (file://) still works, but won't install cleanly or cache offline.

---

## Install it (same link, any device)

- **Android (Chrome):** open the link → menu **⋮** → **Add to Home screen** / **Install app**.
- **iPhone/iPad (Safari):** open the link → **Share** → **Add to Home Screen**.
- **PC (Chrome/Edge):** open the link → click the **Install** icon in the address bar.

It then opens full-screen with the Kampus icon — no browser bars.

---

## Make a real APK (and Windows/iOS packages) — no coding

1. Publish with Option A or B so you have an HTTPS link.
2. Go to **https://www.pwabuilder.com**, paste your link, and press **Start**.
3. It reads this app's manifest + service worker and generates a signed
   **Android package (APK/AAB)** you can install or submit to Google Play,
   plus Windows and iOS packages.

---

## Good to know

- **Install button:** on Android/desktop Chrome an **Install** button appears in the app's
  top bar when the browser allows it (iPhone uses Safari → Share → Add to Home Screen).
- **Previews:** listing photos load from each listing's web page, so they need
  internet. Toggle them in **Finances → Listing previews**.

## Live sync (both of you, same list, in real time)

Optional, and free. It uses **your own Firebase project**, so the data stays in your account.

1. Open **console.firebase.google.com** → **Add project** (any name).
2. **Build → Firestore Database → Create database.**
3. **Project settings (⚙) → Your apps →** web icon **`</>`** → register → copy the
   **firebaseConfig** object.
4. In the app: **Finances → Live sync →** paste the config, choose a **space code**
   (e.g. `frank-and-anke`), tap **Connect & sync**.
5. Firestore → **Rules** → paste and Publish:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{db}/documents {
       match /spaces/{space} { allow read, write: if true; }
     }
   }
   ```
6. Send your wife the app link + the **same space code**; she pastes the same config
   and code. Now edits on one phone appear on the other within a second. A green
   **● Live** badge shows in the top bar when connected.

> The Firebase web config is safe to share (it's public by design). Pick a
> hard-to-guess space code — fine for a private family tool, not bank-grade security.
> Without sync, the app still works fully on-device; back up with Export/Import JSON.

## Updating the app

Re-drag the folder to Netlify (or re-upload to GitHub). If a change doesn't show,
bump `CACHE = 'kampus-v1'` to `'kampus-v2'` in `service-worker.js` so devices fetch
the new version.
