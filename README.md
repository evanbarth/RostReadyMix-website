# Rost Ready Mix | Corporate Website

Corporate website developed and deployed for Rost Ready Mix, an RMCMO company and concrete supplier serving southwest Missouri.

<p align="center">
<img width="760" height="428" alt="rost-website-tour_1" src="https://github.com/user-attachments/assets/4b53f1e5-3bd0-469f-9771-95b795fa675b" />
</p>

## Project Overview

This project covered the full lifecycle of the company's public website: design, development, deployment, and ongoing maintenance using GitHub and Microsoft Azure.

The goal was a site that feels as premium as the brand. It also had to do real work for customers, job seekers, and the office. Contractors can estimate yardage and reach dispatch in one tap. New customers can download credit applications and tax-exempt forms. Applicants can find open positions and the right employment application. The office can post jobs and update details without a developer.

The site is fully static. There is no build step, no framework, no database, and no trackers or analytics. The only third-party script is Google Maps for the plant map, and it loads only when a visitor scrolls near that section.

## Highlights

### Concrete calculators
- **Five calculators:** slab or driveway, footing, wall, pier or column, and steps.
- **Order-ready results:** each result includes a selectable waste allowance and rounds up to the next quarter yard, the way concrete is actually ordered. It also shows the estimated truckloads, and the same job in 80-lb bags for homeowners comparing ready-mix with premix.
- **Project totals:** *Add to Project* builds a running list of every part of a job (for example, footings + slab + steps) and totals it into one order. Volumes are summed before rounding, so multi-part jobs aren't over-rounded.
- **"What Dispatch Will Ask For":** a checklist sits directly under the calculator, so callers are ready with the job address, mix, placement method, and timing.
- **Keyboard support:** the calculator tabs follow the ARIA tabs pattern, with arrow keys, Home, and End. Quick-pick buttons cover common thicknesses.
- **Phones:** a pinned total bar keeps the result in view while the visitor scrolls back up through the form.

### Plant locations map
- **Three companies, one map:** every plant for Rost Ready Mix and its affiliates, Kay Concrete Materials and Joplin Concrete Company, on an embedded Google Map.
- **Custom company markers:** each company has its own mark, drawn in SVG: an orange diamond with an italic "R" for Rost, a red "K" badge for Kay, and a blue "J" badge for Joplin Concrete. The same marks appear in the map key and the location list, so the three always match.
- **Driven by the location list:** the map reads the plant list on the page (name, address, Directions link, coordinates). Adding a plant to the list adds its marker, with nothing to change in the script.
- **Map and Directions:** each plant has a *Map* button that brings the map into view and opens that plant's card, plus a Directions link to its Google Maps listing.
- **Loads only when needed:** Google's script isn't requested until the visitor scrolls near the map, so it adds nothing to the first page load. One-finger and scroll-wheel gestures keep scrolling the page instead of getting caught in the map.
- **Fails gracefully:** if the map can't load, it shows a short message and the Directions links still work.
- **Locked-down API key:** the key works only on the company's domains and only for the Maps JavaScript API.

### Careers
- **Jobs live in one data file:** office staff add a listing by copying an entry in `jobs.js`. There's no HTML to edit and no CMS to maintain.
- **Drafts:** a listing marked as a draft is hidden from the public. Adding `?drafts=1` to the URL previews it with a banner.
- **Automatic closing:** a listing with a closing date removes itself the day after.
- **Shareable links:** each listing has its own link that opens it expanded.
- **Location filters:** filter buttons appear automatically when open positions span more than one plant.
- **The right application:** each listing links to the correct one, CDL or non-CDL. The site detects CDL roles from the listing itself if the field isn't set.

### Customer Center
- **Downloads:** business and individual credit applications, and links to Missouri's tax-exemption forms (Forms 149 and 5060).
- **ACH remittance details are never published.** Customers submit a request, and accounting verifies it by phone before sharing banking information. This is a deliberate guard against payment-redirection fraud and business email compromise.
- **Request form:** it posts to any form endpoint (such as Power Automate or Formspree). If no endpoint is set, it falls back to a pre-filled email to accounting. A hidden honeypot field filters out basic spam bots.

### Hero video
- **Drone footage of the Rogersville plant:** it was stabilized with FFmpeg (`vidstab`) and edited into a seamless loop with quick dissolves.
- **Two sizes:** 960px and 1600px versions are encoded, and the browser loads the one that fits the screen.
- **Respects visitor settings:** visitors who prefer reduced motion, or who have data saver turned on, see a still image instead of the video.
- **Saves resources:** the video pauses when scrolled out of view, and a pause button is always available.

## How It Works

```
index.html                 Homepage: concrete, plants map, calculators, customer center, careers, contact
customer/index.html        Customer Center  (www.rostinc.com/customer/)
careers/index.html         Careers          (www.rostinc.com/careers/)
404.html                   Page-not-found page, shown for any missing address
staticwebapp.config.json   Azure hosting rules: caching, the 404 page, security headers
robots.txt, sitemap.xml    Search engine crawling and indexing
assets/css/site.css        All styles; brand colors defined once as CSS custom properties
assets/js/site.js          Shared settings and behavior (phone numbers, plants, calculator assumptions)
assets/js/home.js          Section highlighting, hero video, calculators, project list
assets/js/plant-map.js     Google Map built from the plant list on the page
assets/js/careers.js       Renders job listings, filters, and deep links
assets/js/jobs.js          Job listings (the only file edited to post a job)
assets/fonts/              Archivo variable font, self-hosted
forms/                     Downloadable PDFs
```

- **One source of truth for business details:** phone numbers and calculator assumptions (truck capacity, ordering increment, bag yield) live in a single `SITE` settings object. Every phone link on every page is tagged with which line it dials and is filled in from those settings. Changing a number means editing one line. The plant list on the homepage likewise feeds both the list and the map.
- **Clean URLs without a server:** each page is an `index.html` inside its own folder, so `/customer/` and `/careers/` work on any static host. When the files are opened straight from a computer for local preview, a small script rewrites those folder links so they still work.
- **Navigation that tracks the page:** the header menu highlights the section currently being read. Menu links land with the section title just below the sticky header.

## Hosting and Deployment

- **Azure Static Web Apps:** GitHub Actions deploys every push to `main`, and every pull request gets its own preview site for review before it goes live.
- **Caching tuned per file type:** pages, styles, scripts, and PDFs revalidate on every visit, so updates appear right away. Fonts are cached for a year, and photos and video for a week.
- **Custom 404 page:** any missing address, at any depth, returns a real 404 status with a branded page that links back to the main sections.
- **Security headers:** HTTPS with HSTS, no framing by other sites, and camera, microphone, and location access turned off. Hosting rules and project notes aren't served publicly.
- **One canonical domain:** `rostinc.com` and plain `http` redirect to `https://www.rostinc.com`, which matches the canonical URLs and the sitemap.

## Performance, Accessibility, and SEO

- **Light page weight:** the homepage loads about 450 KB across 9 requests, including fonts and images, before the hero video. The video and the map load separately, and only when they're actually needed.
- **Self-hosted variable font:** two font files cover every weight, width, and italic used on the site, with no requests to Google Fonts.
- **Responsive images:** served as WebP with JPEG fallbacks, sized to the screen, and lazy-loaded below the fold.
- **Phone layout:** a dedicated layout tightens spacing, puts short items side by side, and keeps tap-to-call one tap away. Tested down to 320px wide.
- **Accessibility:** semantic landmarks and an ordered heading outline, labeled form fields with high-contrast borders, focus outlines colored to stay visible on every background, keyboard-operable tabs, an Escape key that closes the phone menu and returns focus to its button, screen-reader text for icon-only and repeated links, and respect for `prefers-reduced-motion`.
- **SEO:** canonical URLs, Open Graph and Twitter Card tags with full image URLs for link previews, an XML sitemap and `robots.txt`, and `LocalBusiness` structured data (schema.org JSON-LD).

## Technologies & Tools

- HTML, CSS, and vanilla JavaScript (no frameworks or dependencies)
- SVG for icons, brand marks, and map markers
- Google Maps JavaScript API, with a domain- and API-restricted key
- Git and GitHub for version control
- Azure Static Web Apps for hosting, with routing, caching, and header rules
- GitHub Actions for automated deployment (CI/CD) and pull request previews
- Custom domain configuration and HTTPS
- FFmpeg for video stabilization, looping, and encoding
- Python (`pypdf`) for assembling the downloadable application packets
- Playwright for screenshot testing across desktop, tablet, and phone sizes

## Live Website

[Visit Rost Ready Mix](https://www.rostinc.com)

## Repository Information

This repository is a public project showcase. The production source code is maintained separately in a private company-owned repository.
