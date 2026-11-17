# Center for Human Illumination — Website

A static, self-contained site: one `index.html`, an `assets/` folder, and the white paper PDF. No build step or framework.

## Files

- `index.html` — the whole site: HTML, CSS and JavaScript in one file
- `assets/` — logo, Index wheel, white paper cover, co-founder headshots. `logo-full-light.png` and `logo-mark-light.png` are copies of the logos with the glow and orange flares removed and the gold deepened so it reads on ivory, used in light mode.
- `white-paper.pdf` — the download offered after the white paper form is submitted

## Pages

There are eight pages, routed by hash in a single file:

`#home`, `#about`, `#co-founders`, `#the-index`, `#certified`, `#organizations`, `#research`, `#contact`

Each page is a `<div class="page" data-page="...">` block. A small script at the bottom of the file shows the page that matches the URL hash.

## Deploying (Netlify, from GitHub)

The repo lives at github.com/warren-sadler/chi-website. `netlify.toml` sets the publish directory to the repo root (no build command) and adds headers: the white paper PDF is marked `noindex` so it doesn't show up in search results.

1. In Netlify, choose **Add new site → Import an existing project** and pick `chi-website`. Leave the build command empty; the publish directory comes from `netlify.toml`. Every push to `main` redeploys.
2. **Forms:** go to **Site configuration → Forms** and enable form detection, then redeploy. Without this, Netlify ignores the forms. The deploy log should then list both forms: `whitepaper` (name, email) and `contact` (name, email, organization, interest, message). Submissions appear under **Forms** in the dashboard. Turn on email notifications there, and connect them to Mailchimp or HubSpot via Zapier if needed.
3. **Domain:** in Site settings → Domain management, add the domain bought on GoDaddy, then update its DNS at GoDaddy as Netlify instructs.

Vercel or Cloudflare Pages also work, but their form handling would need a replacement (for example Formspree or a small serverless function), since the form markup is Netlify-specific.

## Form behavior

- In a framed preview (such as the claude.ai artifact), forms only show the thank-you state and send nothing.
- On a live, unframed site, forms POST to `/` (Netlify Forms), and the white paper form then reveals a "Download it now" link to `white-paper.pdf`.

## Suggested improvements when rebuilding

- **SEO:** split the hash-routed pages into separate HTML files or routes (for example /about, /the-index) with their own titles and meta descriptions, so each page can be indexed.
- **Analytics:** add Plausible or GA4.
- **Social sharing:** add an Open Graph image (the white paper cover works well) and a favicon (the CHI monogram).
- **Gating:** to keep the PDF from being shared directly, serve it from a signed or expiring URL, or email it after signup instead of linking to it.
- **Headshot:** Warren's photo is only 200×200 px; swap in a larger original.
- **Light-mode artwork:** the light logos were generated from the dark-background PNGs, and the Index wheel is shown on a dark disc in light mode. Proper light-background exports from the brand files would look cleaner.

## Brand

- Colors: background #110010, plum #2A0F24 / #3D1735, mauve #7A4A6C, gold #D4A64A, gold highlight #F2CF7A, champagne #F3E3BF, muted #A8909F
- Fonts (Google Fonts): Source Serif 4 for headings, Inter for body text

## Light and dark themes

The site follows the visitor's OS setting (`prefers-color-scheme`) until they use the sun/moon toggle in the nav; their choice is then saved in `localStorage` under `theme`. A small script in `<head>` sets `data-theme` on `<html>` before the page paints, so there is no flash of the wrong theme.

All colors are CSS variables in `:root` (dark) and `[data-theme="light"]`. Translucent colors use RGB-triplet variables such as `rgba(var(--plum-rgb),.55)`, so each rule keeps its own opacity while the theme swaps the color. Avoid hardcoding colors in new rules.

- Light palette: background ivory #FBF6EC, surfaces #F6EEDF / #F3E8D4, text #3A1A33, headings #2A0F24, muted #6E5566, gold text #7E5B14 / #7A5810 (darkened to meet WCAG AA on ivory). Gold fills such as buttons and the Platinum tile keep the bright brand gold.
