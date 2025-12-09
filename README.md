# Kiln Kissed Ceramics Website

This repository contains the public website for **Kiln Kissed Ceramics by Bhavya Kothari** — a small-batch, handmade ceramics studio based in Sunnyvale, CA. The site is a simple, static site built with HTML and CSS, and is deployed via GitHub Pages.

You can visit the live site here:  
https://kilnkissedceramics.github.io

---

## What's in this repo

This repo only contains the **public-facing website files**. There is **no private customer data, no orders, and no internal tooling** stored here.

Typical structure:

- `index.html` – Home page (overview, hero imagery, where to buy in person)
- `gallery.html` – Photo gallery of recent work with a lightbox viewer
- `shop.html` – Information on how to shop / where to buy
- `about.html` – About Bhavya and the Kiln Kissed Ceramics story
- `contact.html` – Contact form and social links
- `thankyou.html` – Simple thank-you page after form submission
- `style.css` – Shared site-wide styles
- `robots.txt` – Basic crawler instructions
- `sitemap.xml` – Sitemap for search engines (kept in sync with the live pages)
- `assets/` – Images and other static assets

Because this is a static site, there is **no build step** and no backend code.

---

## Lightbox & gallery behavior

The gallery page includes a simple lightbox with:

- Click-to-open larger image view
- **Next/Previous arrows** to move between images
- **Looping navigation** (after the last image, “next” returns to the first; “previous” from the first goes to the last)
- Keyboard support:
  - `Esc` closes the lightbox
  - Left / Right arrow keys move between images

The lightbox is implemented with a small amount of vanilla JavaScript, and no external UI framework.

---

## Analytics & Tag Management (high level)

The site uses **Google Tag Manager (GTM)** to send data to **Google Analytics 4 (GA4)**.

High-level setup (without exposing sensitive IDs):

- A single GTM container is loaded on **all pages** via the standard GTM `<script>` and `<noscript>` snippets.
- Inside GTM, a **Google tag** is configured with the GA4 Measurement ID for this site and set to fire on **All Pages**.
- The gallery lightbox sends structured events into `dataLayer` (e.g. `gallery_lightbox_open`, `gallery_lightbox_navigate`, `gallery_lightbox_close`), which GTM forwards to GA4 via GA4 event tags.

All analytics configuration (tags, triggers, variables) lives in GTM, **not** in this repo.

---

## Running the site locally

Because this is a static site, you can view it locally with any simple HTTP server:

From the repo root:

```bash
# Python 3
python -m http.server 8000

# or Node.js (if installed)
npx serve
```

Then open:

- `http://localhost:8000/index.html`

in your browser.

> **Note:** Some browser privacy settings / extensions (especially in Firefox with Enhanced Tracking Protection) may block GTM/GA4 requests. This only affects analytics, not the basic site layout.

---

## Deployment

This repo is intended to be deployed via **GitHub Pages**:

- The `main` (or configured) branch is used as the GitHub Pages source.
- Changes to the HTML/CSS files here are picked up by GitHub Pages and reflected on the live site after push.

There is no additional build pipeline or deployment tooling.

---

## Contributing

This repository primarily serves as the **source of the live site for Kiln Kissed Ceramics**. It is not currently open to outside contributions.

If you spot a typo, broken link, or accessibility issue and would like to help:

- You can open an issue on the repo, or
- Reach out directly through the contact details on the website.

---

## Content & Usage

All copy, design, and images in this repo are for **Kiln Kissed Ceramics**.

- © Bhavya Kothari. All rights reserved.
- Please **do not reuse photos or text** without explicit permission.

If you’d like to feature or share Kiln Kissed Ceramics, please link to the public site rather than copying content directly.
