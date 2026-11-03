# Rost Ready Mix | Corporate Website

Corporate website developed and deployed for Rost Ready Mix, a concrete supplier serving southwest Missouri.

<p align="center">
  <img src="https://github.com/user-attachments/assets/41f21818-cdad-4531-a1e7-c38d7080a628" alt="Rost Ready Mix homepage" width="100%">
</p>
<p align="center">
  <img src="https://github.com/user-attachments/assets/ea4d94be-d0b7-4379-ba3f-8bceb7f3e996" alt="Rost Ready Mix website, additional view" width="100%">
</p>

## Project Overview

This project covered the full lifecycle of the company's public website: design, development, deployment, and ongoing maintenance using GitHub and Microsoft Azure.

The goal was a site that feels as premium as the brand. It also had to do real work for customers, job seekers, and the office. Contractors can estimate yardage and reach dispatch in one tap. New customers can download credit applications and tax-exempt forms. Applicants can find open positions and the right employment application. The office can post jobs and update details without a developer.

The site is fully static. There is no build step, no framework, no database, and no third-party scripts, trackers, or cookies.

## Highlights

### Concrete calculators
- **Five calculators:** slab or driveway, footing, wall, pier or column, and steps.
- **Order-ready results:** each result includes a selectable waste allowance and rounds up to the next quarter yard, the way concrete is actually ordered. It also shows the estimated truckloads, and the same job in 80-lb bags for homeowners comparing ready-mix with premix.
- **Project totals:** *Add to Project* builds a running list of every part of a job (for example, footings + slab + steps) and totals it into one order. Volumes are summed before rounding, so multi-part jobs aren't over-rounded.
- **"What Dispatch Will Ask For":** a checklist sits directly under the calculator, so callers are ready with the job address, mix, placement method, and timing.
- **Keyboard support:** the calculator tabs follow the ARIA tabs pattern, with arrow keys, Home, and End. Quick-pick buttons cover common thicknesses.
- **Phones:** a pinned total bar keeps the result in view while the visitor scrolls back up through the form.

### Plant locations map
- **Hand-built SVG map:** it shows every plant across both companies, Rost Ready Mix and its affiliate Kay Concrete Materials. Each company has its own marker: an orange diamond for Rost and a red "K" badge for Kay.
- **Plotted from real coordinates:** plants and highways are placed from latitude and longitude with a simple projection, so distances are drawn to scale. Interstate, US, and state route shields are drawn in SVG as well.
- **No map service:** there's no embedded Google Maps, so there's no API key, no tracking, and no extra page weight. Each plant links to its Google Maps listing for turn-by-turn directions.

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
- **Drone footage of the Marshfield plant:** it was stabilized with FFmpeg (`vidstab`) and edited into a seamless loop with quick dissolves.
- **Two sizes:** 960px and 1600px versions are encoded, and the browser loads the one that fits the screen.
- **Respects visitor settings:** visitors who prefer reduced motion, or who have data saver turned on, see a still image instead of the video.
- **Saves resources:** the video pauses when scrolled out of view, and a pause button is always available.

## How It Works

```
index.html               Homepage: concrete, plants map, calculators, customer center, careers, contact
customer/index.html      Customer Center  (rostinc.com/customer)
careers/index.html       Careers          (rostinc.com/careers)
assets/css/site.css      All styles; brand colors defined once as CSS custom properties
assets/js/site.js        Shared settings and behavior (phone numbers, plants, calculator assumptions)
assets/js/home.js        Section highlighting, hero video, calculators, project list
assets/js/careers.js     Renders job listings, filters, and deep links
assets/js/jobs.js        Job listings (the only file edited to post a job)
assets/fonts/            Archivo variable font, self-hosted
forms/                   Downloadable PDFs
```

- **One source of truth for business details:** phone numbers, plant addresses, and calculator assumptions (truck capacity, ordering increment, bag yield) live in a single `SITE` settings object. Every phone link on every page is tagged with which line it dials and is filled in from those settings. Changing a number means editing one line.
- **Clean URLs without a server:** each page is an `index.html` inside its own folder, so `/customer` and `/careers` work on any static host. When the files are opened straight from a computer for local preview, a small script rewrites those folder links so they still work.
- **Navigation that tracks the page:** the header menu highlights the section currently being read. Menu links land with the section title just below the sticky header.

## Performance, Accessibility, and SEO

- **Light page weight:** the homepage loads about 430 KB across 8 requests, including fonts and images, before the hero video. The video is loaded separately, and only when it will actually play.
- **Self-hosted variable font:** two font files cover every weight, width, and italic used on the site, with no requests to Google Fonts.
- **Responsive images:** served as WebP with JPEG fallbacks, sized to the screen, and lazy-loaded below the fold.
- **Phone layout:** a dedicated layout tightens spacing, puts short items side by side, and keeps tap-to-call one tap away.
- **Accessibility:** semantic landmarks and headings, labeled form fields, visible focus styles, keyboard-operable tabs, screen-reader text for icon-only and repeated links, and respect for `prefers-reduced-motion`.
- **SEO:** canonical URLs, Open Graph tags for link previews, and `LocalBusiness` structured data (schema.org JSON-LD).

## Technologies & Tools

- HTML, CSS, and vanilla JavaScript (no frameworks or dependencies)
- SVG for the map, icons, and brand marks
- Git and GitHub for version control
- Azure Static Web Apps for hosting
- GitHub Actions for automated deployment (CI/CD)
- Custom domain configuration and HTTPS
- FFmpeg for video stabilization, looping, and encoding
- Python (`pypdf`) for assembling the downloadable application packets
- Playwright for screenshot testing across desktop, tablet, and phone sizes

## Live Website

[Visit Rost Ready Mix](https://rostinc.com)

## Repository Information

This repository is a public project showcase. The production source code is maintained separately in a private company-owned repository.
