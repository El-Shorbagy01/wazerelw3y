# Publishing wazerelw3y on GitHub

This blog needs two things set up before it works online: a **Firebase database**
(so everyone sees the same posts) and **GitHub Pages** (to host the site itself).
No coding tools or build steps needed — it's one plain `index.html` file.

## 1. Create the free Firebase database (~5 minutes)

1. Go to https://console.firebase.google.com and sign in with a Google account.
2. Click **Add project** → name it anything (e.g. `wazerelw3y`) → finish the wizard.
3. In the left sidebar, click **Build → Firestore Database** → **Create database**.
   - Choose **Start in test mode** for now (easiest to get running; see step 4 for making it safer later).
   - Pick any region close to you.
4. Once created, go to **Project settings** (gear icon, top left) → scroll to **Your apps**
   → click the **</>** (web) icon → register the app (any nickname) →
   **do not** check "Firebase Hosting".
5. Firebase shows you a `firebaseConfig` object. Copy those values.
6. Open `index.html` in this folder, find this block near the top, and paste your real values in:
   ```js
   const firebaseConfig = {
     apiKey: "YOUR_API_KEY",
     authDomain: "YOUR_PROJECT.firebaseapp.com",
     projectId: "YOUR_PROJECT_ID",
     storageBucket: "YOUR_PROJECT.appspot.com",
     messagingSenderId: "YOUR_SENDER_ID",
     appId: "YOUR_APP_ID"
   };
   ```

**Optional but recommended — lock down who can delete posts:**
Test mode lets *anyone* read and write for 30 days, then it locks automatically.
For a public blog, go to **Firestore → Rules** and set:
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /posts/{postId} {
      allow read: if true;
      allow create: if request.resource.data.title is string
                    && request.resource.data.body is string;
      allow delete: if true; // anyone with the link can delete — tighten later if needed
    }
  }
}
```
This is still open (no login system), matching what you asked for — anyone with the
link can post and delete. If you later want only *you* to delete posts, tell me and
I'll add a simple password gate.

## 2. Put the code on GitHub

1. Go to https://github.com/new
   - Repository name: `wazerelw3y` (or anything)
   - Public
   - Create repository
2. On the new repo's page, click **uploading an existing file**.
3. Drag in `index.html` (the edited one, with your Firebase values in it).
4. Commit the file.

## 3. Turn on GitHub Pages

1. In your repo, go to **Settings → Pages**.
2. Under **Build and deployment → Source**, choose **Deploy from a branch**.
3. Branch: `main`, folder: `/ (root)` → **Save**.
4. Wait ~1 minute, then refresh — GitHub shows your live URL, something like:
   `https://yourusername.github.io/wazerelw3y/`

That link is now your working, shared, bilingual blog.

## 4. (Optional) Use your own domain — wazerelw3y.com

1. Buy the domain from any registrar (Namecheap, GoDaddy, etc).
2. In the repo, **Settings → Pages → Custom domain** → enter `wazerelw3y.com` → Save.
   GitHub creates a `CNAME` file in your repo automatically.
3. At your domain registrar, add these DNS records (exact steps vary by registrar):
   - Four `A` records for `@` pointing to:
     `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - One `CNAME` record for `www` pointing to `yourusername.github.io`
4. DNS changes can take up to a few hours to apply. Once they do, check
   **Settings → Pages** again and enable **Enforce HTTPS**.

---

If any step throws an error, copy the exact message back to me and I'll help debug it.
