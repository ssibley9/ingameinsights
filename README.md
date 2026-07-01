# InGameInsights Website

Static website for [ingameinsights.com](https://ingameinsights.com) — landing page and how-to guides for the InGameInsights stat tracking apps: Soccer, Hoops '25, and LAX '26.

## File structure

```
/
├── index.html            Main landing page (hero, app lineup, about)
├── guides.html           Guides hub — links to the three app guides
├── guide-soccer.html     How-to guide: Soccer
├── guide-hoops.html      How-to guide: Hoops '25
├── guide-lax.html        How-to guide: LAX '26
├── privacy.html          Privacy policy
├── support.html          Support / FAQ
├── 404.html              Fallback for missing pages
├── CNAME                 GitHub Pages custom domain config
└── assets/
    ├── logo.png                       Favicon — monogram on a pine tile
    ├── ingameinsights-mark-reversed.svg  Nav + hero mark (cream lines, gold arc)
    ├── ingameinsights-mark.svg        Brand mark, source (pine + gold)
    ├── ingameinsights-appicon.svg     Brand app icon, source
    ├── ingameinsights-wordmark.svg    Brand wordmark, source
    ├── app-store-badge.svg            Apple "Download on the App Store" badge
    ├── soccer-icon.png                Soccer app icon
    ├── hoops-icon.jpg                 Hoops '25 app icon
    ├── lax-icon.png                   LAX '26 app icon
    └── guide-<sport>-<n>.png          Guide screenshots (added as available)
```

## Brand

The site uses a shared design system, defined as CSS variables in the `:root` block of each page. If you change a brand color, update it in every HTML file's `:root` (the values are duplicated per file).

```
--field-deep  #072a20   darkest green — cards, about section
--field       #0b3d2e   page background (brand pine)
--field-mid   #12503c   hover fills
--field-line  #1d5d46   borders
--cream       #efdc99   primary text
--cream-soft  #d4c285   secondary text
--cream-mute  #8a7d5a   muted labels
--gold        #b8860b   primary accent, CTAs (brand gold)
--gold-deep   #8f6708   gold borders / hover
--rose        #e8645a   "In Beta" accent
```

Fonts (Google Fonts CDN): Anton (display), DM Serif Display (headings), Inter (body).

The logo is a linked "IGI" monogram, supplied as SVG. On the dark site it appears as `ingameinsights-mark-reversed.svg` (cream lines, gold arc). The favicon `logo.png` is rendered from the app-icon SVG. The raw brand SVGs are kept in `assets/` as source files for future exports.

## Guides

Each app has its own guide page with numbered steps and a screenshot per step. `guides.html` is the hub; each app card on the homepage also links directly to its guide.

Screenshots are wired to predictable filenames — `guide-soccer-1.png` … `guide-lax-4.png`. Until a file exists, the step shows a styled "add this file" placeholder instead of a broken image, so add them whenever you're ready. Use lowercase `.png` names that match exactly. iPad screenshots are `.png` by default; the frame is responsive, so landscape or portrait both work.

## Deployment to GitHub Pages

The site auto-deploys on every commit to `main`. There is no separate publish step — commit and the change goes live in ~1 minute.

### DNS (already configured, for reference)

The apex domain points at GitHub Pages via four A records, with `www` as a CNAME. In GoDaddy → DNS:

| Type | Name | Value |
|------|------|-------|
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |
| CNAME | www | YOURUSERNAME.github.io |

"Enforce HTTPS" is enabled in Settings → Pages (GitHub provisions the Let's Encrypt cert automatically once DNS resolves).

## Updating the site

1. Edit or upload a file directly in the GitHub web interface.
2. Scroll down and click **Commit changes** (edits and uploads only save if you commit).
3. GitHub Pages auto-deploys within ~1 minute.
4. Hard-refresh the live site (Cmd/Ctrl+Shift+R) or use a private window — GitHub's CDN and your browser both cache aggressively.

If you replace an image and the old one persists, either hard-refresh or bump the reference with a query string (e.g. `assets/soccer-icon.png?v=2`) to force browsers to re-fetch.

## Common updates

- **Adding guide screenshots**: drop `guide-<sport>-<n>.png` files into `assets/`. No HTML change needed.
- **Hoops '25 or LAX '26 launches**: in `index.html`, change that app's card `app-card-status` from `in-beta` to `available`, and swap the "Email for TestFlight access" link for the App Store badge (copy the pattern from the Soccer card). Update the matching guide page's status pill and remove its beta note.
- **New app icon**: replace the file in `assets/` (keep the same filename), commit. Done.
- **Brand color tweak**: update the value in the `:root` block of every HTML file so all pages stay in sync.

## Notes

- All animations respect `prefers-reduced-motion`.
- The dot-grid texture is a CSS radial-gradient, not an image — crisp at any zoom.
- No analytics, no tracking, no third-party scripts beyond Google Fonts.
- The apps collect no user data; everything stays on the device. See `privacy.html`.
```
