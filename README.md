# Astrotalk Creator Program — Onboarding Landing Page

A single-file landing page (`index.html`) with a 3-step influencer application
form (Details → Portfolio → Confirmation), built in Astrotalk's dark style: black background, `#F0DF20` yellow accents,
Inter throughout (light weights for headlines, bold yellow for the key words),
modelled on Astrotalk's own psychic landing pages.

**Layout:** the application form sits directly in the hero, side-by-side with
the pitch, so a visitor can start filling it in immediately with no scrolling
on desktop. On mobile the form comes straight after the headline, with the
benefits and social stats below it. A sticky "Apply" bar appears on mobile only
once the form has scrolled out of view.

Sections: Hero + form → Trusted by Millions (88M+ users, 120M+ downloads,
7+ countries; users and downloads taken from Astrotalk's existing landing page) →
How it works → Live Instagram posts → Final "limited spots" CTA → Footer.

## What's in here

- `index.html` — the full landing page + form. No build step, just open it or host it.
- `assets/astrotalk-logo.svg` — the Astrotalk logo, used in the header, the
  footer and as the browser-tab icon. It's a vector redraw of the logo PNG; if
  you'd rather use the original file, save it as `assets/astrotalk-logo.png` and
  change the three `astrotalk-logo.svg` references in `index.html`.
- `apps-script/Code.gs` — Google Apps Script backend that writes form submissions
  into a Google Sheet and saves uploaded photos to Google Drive.

## How the form flow works

1. **Step 1 (Your Details):** Name, age, gender, mobile. On "Continue", this is
   sent to your Google Sheet immediately and a `submissionId` is created.
2. **Step 2 (Portfolio):** 2 photo uploads, past gigs, portfolio/Instagram link,
   comments. On submit, this updates the *same row* in the sheet using the
   `submissionId`, and uploads photos to a Drive folder.
3. **Step 3:** Confirmation screen with next steps.

If you haven't wired up the backend yet, the form still works end-to-end in
"demo mode" (it just simulates the save) so you can test the UI immediately.

## Connecting it to Google Sheets (one-time setup)

1. Create a new Google Sheet (or open the one you want submissions to land in).
2. In the Sheet, go to **Extensions → Apps Script**.
3. Delete the placeholder code and paste in the entire contents of
   `apps-script/Code.gs`.
4. In the Apps Script editor, select the `setupSheet` function from the
   function dropdown at the top, and click **Run**. The first time, Google
   will ask you to authorize the script — click through and allow it (it's
   your own script acting on your own Sheet/Drive).
5. Click **Deploy → New deployment**.
   - Click the gear icon next to "Select type" and choose **Web app**.
   - **Execute as:** Me
   - **Who has access:** Anyone
   - Click **Deploy**, authorize again if asked.
6. Copy the **Web app URL** you're given (looks like
   `https://script.google.com/macros/s/XXXXXXXX/exec`).
7. Open `index.html`, find this line near the bottom (inside the `<script>` tag):

   ```js
   const APPS_SCRIPT_URL = 'PASTE_YOUR_APPS_SCRIPT_WEB_APP_URL_HERE';
   ```

   and replace the placeholder with the URL you copied.
8. Save and reload the page — submissions will now land in your Sheet, and
   photos will be saved to a Drive folder called
   **"Astrotalk Creator Program - Photos"**, with the file links written into
   the sheet.

### Sheet columns created automatically

| Timestamp | Submission ID | Name | Age | Gender | Mobile | Photo 1 URL | Photo 2 URL | Past Gigs / Brands | Portfolio / Instagram / YouTube | Comments | Status |
|---|---|---|---|---|---|---|---|---|---|---|---|

> Whenever you update the Apps Script code later, you'll need to create a
> **new deployment version** (Deploy → Manage deployments → Edit → New
> version) for the changes to take effect on the same URL.

## Creator Stories section (real Instagram/Facebook embeds)

The "Creator Stories" carousel (`#carouselTrack`) now shows **live, real
embeds**, not mockups:

- The 3 Instagram posts render via Instagram's official `embed.js` — the
  likes/comments/views shown are pulled live from Instagram, and they're
  actually playable.
- The Facebook reel link you sent (`facebook.com/reel/1214339270604282`)
  **can't be embedded** — Facebook doesn't support embedding Reels the way it
  supports regular Page videos (neither the `plugins/video.php` iframe nor the
  official `fb-video`/XFBML embed will render a Reel). Instead of showing a
  broken box, that slide is a styled card with a "Watch this reel on
  Facebook ↗" link that opens the real reel in a new tab. If you'd rather have
  a true live embed there, either:
  - swap in a regular (non-Reel) Facebook video post's link, or
  - re-upload that same clip as an Instagram Reel/post and use the Instagram
    embed method instead (that one does work reliably).
- **These embeds need internet access to render** (they load
  `instagram.com/embed.js` at runtime) and only work for posts that are
  public with embedding allowed by the account owner.
- To swap in different posts later: replace the `data-instgrm-permalink` URL
  on each `<blockquote class="instagram-media">` in the "stories" section.

## Still to plug in before launch

- **Google rating**: the 4.6★ in the hero stat bar is still a placeholder. Facebook (600K+) and
  Instagram (3.0M) are real follower counts.
- **Cohort deadline**: "10th October" appears in the hero chip, the final CTA
  and the mobile sticky bar. Search `10th Oct` in `index.html` if it changes.
- **Program name** — currently "Astrotalk Creator Program" throughout. If you
  land on a different name, it's a straightforward find/replace.
- **Domain/hosting** — this is a static file, so it can be hosted anywhere
  (Netlify, Vercel, GitHub Pages, or your existing website's static hosting).

## Testing locally

Just open `index.html` directly in a browser, or serve the folder with any
static server, e.g.:

```bash
cd /Users/astrotalk/Desktop/Influencer_onboarding
python3 -m http.server 8000
```

Then visit `http://localhost:8000`.
