# Kléo — Landing Page (problem-validation)

A one-page site to test demand for **Kléo**. Our core metric:

> **Conversion = waitlist joins ÷ visitors**

If lots of people land and a healthy share join the waitlist, the *problem* resonates —
even before we've built the solution.

---

## How the metric works

GitHub Pages is **static** (no server), so counts can't live in the page itself.
Two free services do the counting; the page is already wired for both — you just paste
two IDs into the `CONFIG` block at the bottom of `index.html`.

| Number | Source | Tool |
|--------|--------|------|
| **Visitors** (the denominator) | page views / users | Google Analytics 4 |
| **Waitlist joins** (+ the emails) | form submissions | Formspree |
| **Conversion rate** = joins ÷ visitors | key-event rate | shown in GA4 |

---

## STEP 1 — Google Analytics 4 (counts visitors + joins)

1. Go to **https://analytics.google.com** → *Admin* (bottom-left gear) → **Create → Account**,
   then **Create Property** (name it "Kléo").
2. Under the property add a **Web** data stream with your future URL
   (`https://<your-user>.github.io/kleo/`).
3. Copy the **Measurement ID** — it looks like `G-XXXXXXXXXX`.
4. Open `index.html`, find the `CONFIG` block near the bottom, and set:
   ```js
   GA4_ID: "G-XXXXXXXXXX",
   ```
5. (For the ratio) In GA4: *Admin → Events*, and once the `newsletter_signup` event has
   fired at least once, toggle **Mark as key event**. GA4 will then show the
   **conversion rate** (joins ÷ visitors) directly.

**Where you read the numbers:** GA4 → *Reports → Realtime* (live) or *Reports → Engagement →
Events*. `page_view` = access; `newsletter_signup` = joins.

## STEP 2 — Formspree (captures the actual waitlist emails)

GA4 counts joins but doesn't give you the email addresses — Formspree does.

1. Go to **https://formspree.io** → sign up (free tier = 50 submissions/month).
2. **New form** → copy its endpoint, e.g. `https://formspree.io/f/abcdwxyz`.
3. In `index.html` `CONFIG`, set:
   ```js
   FORM_ENDPOINT: "https://formspree.io/f/abcdwxyz"
   ```
4. Submissions now land in your Formspree inbox (and can email you on each signup).

> Alternative (unlimited, free): a Google Sheet + Apps Script webhook. Ask if you want that instead.

## STEP 3 — Deploy to GitHub Pages

From this folder in a terminal:

```bash
cd "path/to/Landing Page"
git add .
git commit -m "Kléo landing page"
```

Then create the repo on **https://github.com/new**:
- Repository name: **kleo** · Public · **do not** add a README (we have one).

Copy the two commands GitHub shows under *"…or push an existing repository"*, they look like:

```bash
git remote add origin https://github.com/<your-user>/kleo.git
git branch -M main
git push -u origin main
```

Enable the site: repo → **Settings → Pages** → *Build and deployment* → Source = **Deploy from
a branch** → Branch = **main** / **/(root)** → **Save**. In ~1 minute your page is live at:

```
https://<your-user>.github.io/kleo/
```

## STEP 4 — Updating later

Edit `index.html`, then:

```bash
git add . && git commit -m "update copy" && git push
```

GitHub Pages redeploys automatically in under a minute.

---

## Local testing dashboard (single browser only — NOT the shared metric)

Open `index.html` locally and add `?admin` to the URL (or press **Ctrl+Shift+K**) to see
clicks, signups, and conversion for **your own** browser. Use this to sanity-check tracking
before publishing. The real cross-visitor numbers come from GA4/Formspree (steps 1–2).
