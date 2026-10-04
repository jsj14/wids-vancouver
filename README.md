# WiDS Vancouver — website

A single static page (`index.html`, no build step, no framework). Everything — HTML, CSS, and JS — lives in that one file.

## Before you deploy

1. **The sign-up forms are fully connected**, including email on all three. The Speaker / Volunteer / Sponsor forms on the page silently POST to real Google Forms in the background (no redirect, no Google branding shown) — responses land straight in each form's linked Sheet. This is wired up in `index.html`'s `<script>` block as a `GOOGLE_FORMS` config object, one entry per form, each mapping the page's field names to that Google Form's internal `entry.NNNNNN` ids (Speaker's email uses Google's own built-in email-collection field, `emailAddress`; Volunteer's and Sponsor's are regular questions, `entry.1554892012` and `entry.1673393983`).

   If any of the three Google Forms ever gets rebuilt from scratch (not just edited/added to), its `action` URL and every `entry.*` id changes — the whole `GOOGLE_FORMS` block for that form would need to be re-pulled the same way (open the live form and read `window.FB_PUBLIC_LOAD_DATA_` in the console, or ask Claude to do it).

2. **Update the placeholders.** Search the file for:
   - `hello@widsvancouver.org` and `speakers@widsvancouver.org` — swap in your real inbox(es).
   - `Next edition — date & venue TBA` in the hero, and the matching text in the Register section — update once the date and venue are locked.
   - The tentative schedule times, and the sponsor tier names (no pricing is set).

## Deploy to GitHub + Vercel

**1. Push to GitHub**
```bash
cd wids-vancouver-site        # the folder containing index.html
git init
git add index.html README.md
git commit -m "WiDS Vancouver site"
git branch -M main
git remote add origin https://github.com/<your-username>/wids-vancouver.git
git push -u origin main
```
(Create the empty repo on GitHub first — github.com → New repository.)

**2. Deploy on Vercel**
- Go to [vercel.com/new](https://vercel.com/new) and import the GitHub repo you just pushed.
- Framework preset: choose **Other** (it's a plain static site — no build command, no output directory needed).
- Click **Deploy**. Vercel serves `index.html` at the root automatically.
- Every push to `main` after this redeploys the site.

**3. Custom domain (optional)**
In the Vercel project → Settings → Domains, add `widsvancouver.org` (or whatever you're using) and follow the DNS instructions Vercel gives you.

## Notes

- No build tools, no `package.json`, no dependencies to install — it's just HTML/CSS/JS.
- Fonts load from Google Fonts (Fraunces, Work Sans, IBM Plex Mono) via a `<link>` tag — no local font files needed.
- The page supports light and dark mode automatically based on the visitor's system setting.
