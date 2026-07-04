# Coherence — legal site

Tiny static site with the **Privacy Policy** and **Terms of Service** for the Coherence app.
Separate from the app; the app just links to these hosted pages.

```
index.html     → /            (links to both)
privacy.html   → /privacy
terms.html     → /terms
style.css      → shared styles
vercel.json    → cleanUrls (so /privacy and /terms work without .html)
```

## Before you publish
1. Replace the draft text in `privacy.html` and `terms.html` with your final copy.
2. Replace the placeholders in both files:
   - `[Your Company / Legal Entity]`
   - `[SUPPORT_EMAIL]` (also the `mailto:` links)
   - `[Your jurisdiction]` (terms, governing law)
3. Update the `Last updated` date if you change the content. **Keep it in sync with `LEGAL_VERSION`
   in the app (`coherence/lib/links.ts`)** so the recorded consent version matches the page.

## Deploy to Vercel via GitHub

**1. Create a GitHub repo** (from this folder, `c:\dev\Coherence\legal-site`):
```
cd c:\dev\Coherence\legal-site
git init
git add -A
git commit -m "Coherence legal site (privacy + terms)"
gh repo create coherence-legal --public --source=. --push
```
(Or create an empty repo on github.com and `git remote add origin <url> && git push -u origin main`.
Use `--private` instead of `--public` if you prefer; Vercel works with either.)

**2. Connect to Vercel**
- Go to https://vercel.com/new, sign in, and **Import** the `coherence-legal` GitHub repo.
- Framework preset: **Other** (it's plain static HTML — no build step). Leave build/output settings empty.
- Click **Deploy**.

**3. Your live URLs**
After deploy you'll get a project URL like `https://coherence-legal.vercel.app`. The pages are:
- Privacy: `https://coherence-legal.vercel.app/privacy`
- Terms:   `https://coherence-legal.vercel.app/terms`

(The exact subdomain is whatever Vercel assigns to the project name; you can also add a custom domain later.)

**4. Wire the app** — put the two URLs + your support email into `coherence/.env`:
```
EXPO_PUBLIC_PRIVACY_URL=https://coherence-legal.vercel.app/privacy
EXPO_PUBLIC_TERMS_URL=https://coherence-legal.vercel.app/terms
EXPO_PUBLIC_SUPPORT_EMAIL=you@yourdomain.com
```
The app already falls back gracefully, but these make the in-app Privacy/Terms/Support links open your real
pages. Rebuild/reload the app after changing `.env`.

Every push to the repo's main branch auto-redeploys the site.
