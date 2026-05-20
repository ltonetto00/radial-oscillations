# Getting started

A copy of these steps in case you're new to the workflow.

## 1. Create the GitHub repo

Go to https://github.com/new and create an empty repository called
`stellar-oscillations` (or anything you like). Do **not** initialize it with
a README, .gitignore, or license — we have those already.

## 2. Push the local files

From inside the unzipped folder:

```bash
git init
git add .
git commit -m "Initial commit: relativistic radial oscillations app"
git branch -M main
git remote add origin https://github.com/<your-username>/stellar-oscillations.git
git push -u origin main
```

## 3. Test locally first

Just open `index.html` in your browser — no server needed.

```bash
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
```

The page should load with the 2D slice already animating. If you see CORS
errors in the console related to Three.js, your browser may be blocking the
CDN load over `file://`; serve the file over a tiny local server instead:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## 4. Deploy on Vercel

### Option A — GitHub integration (easiest)

1. Sign in at https://vercel.com (free hobby tier is fine).
2. Click **Add New → Project**.
3. Import your GitHub repo.
4. Framework Preset: **Other**. Build Command: *(leave blank)*.
   Output Directory: *(leave blank)*. Root Directory: `./`.
5. Click **Deploy**. After ~10 seconds you'll get a URL like
   `stellar-oscillations-yourname.vercel.app`.

### Option B — Vercel CLI

```bash
npm install -g vercel
cd stellar-oscillations
vercel              # answer prompts, accept defaults
vercel --prod       # promote to production
```

## 5. After deployment

- Update the `id="repo-link"` href in `index.html` to point to your repo.
- Every push to `main` triggers an automatic redeploy on Vercel.
- The `vercel.json` file is included for future extensibility but isn't
  required.
