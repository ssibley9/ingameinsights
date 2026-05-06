# InGameInsights Website

Static website for ingameinsights.com — landing page for Soccer, Hoops '25, and LAX '26.

## File structure

```
/
├── index.html        Main landing page
├── privacy.html      Privacy policy
├── support.html      Support / FAQ
├── 404.html          Fallback for missing pages
├── CNAME             GitHub Pages custom domain config
└── assets/
    ├── logo.png        InGameInsights composite logo
    ├── soccer-icon.png App icon for Soccer
    ├── hoops-icon.png  App icon for Hoops '25
    └── lax-icon.png    App icon for LAX '26
```

## Deployment to GitHub Pages

### One-time setup

1. **Create a GitHub repository.** Go to https://github.com/new
   - Repository name: anything (e.g., `ingameinsights-site`)
   - Public (required for free GitHub Pages)
   - Don't add README, .gitignore, or license — we want it empty

2. **Upload the files.** Easiest path if you're not comfortable with git:
   - On your new empty repo page, click "uploading an existing file"
   - Drag in everything from this folder (index.html, privacy.html, support.html, 404.html, CNAME, and the entire `assets/` folder)
   - Commit at the bottom of the page

3. **Enable GitHub Pages.**
   - In your repo, go to **Settings** → **Pages** (left sidebar)
   - Under "Build and deployment" → "Source", select **Deploy from a branch**
   - Under "Branch", select `main` and `/` (root), then click **Save**
   - Wait 1–2 minutes — GitHub will tell you "Your site is live at https://YOURUSERNAME.github.io/ingameinsights-site/"

4. **Set up the custom domain.**
   - On the same Settings → Pages page, in the "Custom domain" field, enter `ingameinsights.com` and click Save
   - GitHub will start checking DNS — it'll fail until step 5 is done, that's expected
   - Check the box "Enforce HTTPS" once it becomes available (after DNS verifies)

### DNS configuration in GoDaddy

You need to point the domain at GitHub's servers. In GoDaddy:

1. Sign in to your GoDaddy account
2. Click your name (top-right) → **My Products**
3. Find ingameinsights.com → click **DNS**
4. **Delete any existing A records and CNAME records** for `@` and `www` (these are pointing at GoDaddy's parking page)
5. **Add four new A records** for the apex domain (`@`):

   | Type | Name | Value | TTL |
   |------|------|-------|-----|
   | A | @ | 185.199.108.153 | 600 seconds |
   | A | @ | 185.199.109.153 | 600 seconds |
   | A | @ | 185.199.110.153 | 600 seconds |
   | A | @ | 185.199.111.153 | 600 seconds |

   These four IPs are GitHub Pages' servers. Yes, you need all four for redundancy.

6. **Add a CNAME record** for `www` to redirect to the apex domain:

   | Type | Name | Value | TTL |
   |------|------|-------|-----|
   | CNAME | www | YOURUSERNAME.github.io | 1 hour |

   Replace `YOURUSERNAME` with your actual GitHub username (no trailing slash, no https://).

7. Save. DNS propagation usually takes 10 minutes to an hour, sometimes up to 24 hours.

### Verify it works

After DNS propagates:

- https://ingameinsights.com → loads the site
- https://www.ingameinsights.com → redirects to ingameinsights.com
- Both should have the green padlock (HTTPS via GitHub-issued Let's Encrypt cert)

If HTTPS isn't working after a few hours, go back to Settings → Pages → click "Enforce HTTPS" if it's available. GitHub provisions the certificate automatically once DNS resolves correctly.

## Updating the site

After the initial setup, updating is easy:

1. Edit any file (or upload a new one) directly in the GitHub web interface
2. Commit the change
3. GitHub Pages auto-deploys within ~1 minute
4. Refresh the live site to see changes

If you change image files, browsers may cache the old version — bump the file path with a query string (e.g., `assets/soccer-icon.png?v=2`) to force a refresh.

## Common updates

- **Soccer launches**: edit `index.html`, change the soccer card status from "Coming Soon" to "Available on App Store" and replace the email link with the App Store URL. The `app-card-status` class already supports an `available` state — just add `.available` styles or use `.in-beta` styling temporarily.
- **New blog post / news**: not currently set up. If you want this, let me know and I'll add a simple `news.html` template.
- **New app icon**: replace the file in `assets/` (keep the same filename), commit. Done.

## Notes

- The site uses Google Fonts (Anton, DM Serif Display, Inter). These are loaded from Google's CDN — no fallback. Disable JavaScript / blocked third-party requests will fall back to system fonts.
- The dot-grid texture is a CSS gradient, not an image. Renders crisply on any zoom level.
- All animations respect `prefers-reduced-motion` for users who've disabled motion in their OS settings.
- The site is a single page plus three supporting pages — total page weight is ~600KB including all icons, well under 1MB.
