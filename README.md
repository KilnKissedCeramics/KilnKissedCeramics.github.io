# Kiln Kissed Ceramics Website

This repository contains the public website for **Kiln Kissed Ceramics by Bhavya Kothari** — a small-batch, handmade ceramics studio based in Sunnyvale, CA.

The site is now a **light Jekyll site** deployed with **GitHub Pages**. Jekyll is used to keep shared layout pieces, metadata, navigation, schema, events, and page-specific scripts/includes easier to maintain while still keeping the site simple and static.

Live site:  
https://www.kilnkissedceramics.com

---

## What's in this repo

This repo only contains the **public-facing website files**. There is **no private customer data, no orders, and no internal tooling** stored here.

Typical structure:

- `_config.yml` – Jekyll configuration, including the Events collection
- `_layouts/default.html` – Shared HTML shell for pages
- `_includes/` – Shared and page-specific includes for head metadata, GTM, header, footer, styles, schema, navigation script, and lightboxes
- `_event_data/` – One markdown file per event; used to render the Events page and Event schema
- `index.html` – Home page
- `gallery.html` – Photo gallery of recent work with a lightbox viewer
- `shop.html` – Information on how to shop online/in person
- `about.html` – About Bhavya and the Kiln Kissed Ceramics story
- `contact.html` – Contact form and social links
- `thankyou.html` – Simple thank-you page after form submission
- `events/index.html` – Public Events page, available at `/events/`
- `events/YYYY/event-slug/` – Event image folders
- `style.css` – Shared site-wide styles
- `robots.txt` – Basic crawler instructions
- `sitemap.xml` – Sitemap for search engines, kept in sync with live pages
- `assets/` – Images and other static assets

---

## Jekyll structure

The site uses a small Jekyll setup rather than a heavy theme or framework.

Shared layout and includes:

- `_layouts/default.html` owns the shared page shell:
  - `<!doctype html>` / `<html>` / `<head>` / `<body>`
  - GTM includes
  - shared head metadata
  - header and footer
  - main content wrapper
  - shared navigation script
  - conditional page-specific schema, styles, and lightboxes
- `_includes/head.html` renders shared metadata, favicons, canonical links, Open Graph, Twitter metadata, and the shared stylesheet.
- `_includes/header.html` and `_includes/footer.html` render the shared site chrome.
- `_includes/nav-script.html` handles the mobile navigation drawer.
- Page-specific includes are used where helpful, such as:
  - `_includes/styles-home.html`
  - `_includes/styles-about.html`
  - `_includes/styles-shop.html`
  - `_includes/styles-events.html`
  - `_includes/styles-gallery.html`
  - `_includes/lightbox-home.html`
  - `_includes/lightbox-gallery.html`
  - `_includes/schema-localbusiness.html`
  - `_includes/schema-home.html`
  - `_includes/schema-events.html`
  - `_includes/schema-gallery.html`

Pages use Jekyll front matter to opt into page-specific behavior. Example:

```yaml
---
layout: default
title: Gallery | Kiln Kissed Ceramics Pottery Portfolio
description: Browse the Kiln Kissed Ceramics gallery for a visual overview of vibrant glazes, rich textures, and organic details that give each piece its own unique character.
canonical_url: https://www.kilnkissedceramics.com/gallery.html
schema: gallery
page_styles: gallery
lightbox: gallery
---
```

---

## Events system

The Events page is built with Jekyll data files stored as a collection in `_event_data/`.

Each event has one markdown file, for example:

```text
_event_data/2026-05-30-sjmade-fest.md
```

The public Events page is:

```text
events/index.html
```

which is available at:

```text
/events/
```

Important behavior:

- The main Events page shows active events only.
- An event is treated as active until 2 days after its end date.
- Past events are hidden from the main page after that buffer.
- Event details open in a lightbox.
- The lightbox can navigate through all events, including past events.
- Event schema is generated from `_event_data/` through `_includes/schema-events.html`.
- Events use `dataLayer.push()` analytics events only; GA4 is handled through GTM.

Event images are organized under event-specific folders, for example:

```text
events/2026/sjmade-fest/sjmade-fest-2026.webp
events/2026/ovcag-spring-sale/ovcag-spring-sale-2026.jpg
```

---

## Gallery lightbox behavior

The Gallery page uses `_includes/lightbox-gallery.html` and `_includes/styles-gallery.html`.

The lightbox supports:

- Click-to-open larger image view
- Next/Previous buttons
- Looping navigation
- Keyboard support:
  - `Esc` closes the lightbox
  - Left / Right arrow keys move between images
- `dataLayer.push()` analytics events:
  - `gallery_lightbox_open`
  - `gallery_lightbox_navigate`
  - `gallery_lightbox_close`

Gallery ImageObject structured data is kept in `_includes/schema-gallery.html`.

---

## Analytics & Tag Management

The site uses **Google Tag Manager (GTM)** to send data to **Google Analytics 4 (GA4)**.

High-level setup:

- GTM is loaded through shared Jekyll includes:
  - `_includes/gtm-head.html`
  - `_includes/gtm-body.html`
- GTM is included by `_layouts/default.html`, so it loads consistently across pages that use the shared layout.
- Do **not** add direct GA4 / `gtag.js` snippets to individual pages.
- Page interactions should use `dataLayer.push()`.
- GTM handles mapping those `dataLayer` events to GA4.

Current lightbox/event interaction examples include:

- `gallery_lightbox_open`
- `gallery_lightbox_navigate`
- `gallery_lightbox_close`
- `event_lightbox_open`
- `event_lightbox_navigate`
- `event_lightbox_close`
- `event_external_click`

All analytics configuration, tags, triggers, and variables live in GTM, **not** in this repo.

---

## Running the site locally

This site should be previewed with Jekyll, not a plain static file server.

Ruby is managed with `rbenv`. The repo uses a repo-local Ruby version file:

```text
.ruby-version
```

Expected Ruby version:

```text
3.1.6
```

From the repo root:

```bash
bundle exec jekyll serve
```

Then open:

```text
http://localhost:4000/
```

Useful local URLs:

```text
http://localhost:4000/
http://localhost:4000/gallery.html
http://localhost:4000/shop.html
http://localhost:4000/about.html
http://localhost:4000/contact.html
http://localhost:4000/events/
```

> Note: Some browser privacy settings or extensions may block GTM/GA4 requests. This only affects analytics testing, not the basic site layout.

---

## Deployment

This repo is deployed via **GitHub Pages**.

- GitHub Pages builds the site with Jekyll.
- The `main` branch is used as the source unless configured otherwise in GitHub Pages settings.
- Changes pushed to the repo are built by GitHub Pages and reflected on the live site after the build completes.
- `.nojekyll` should not be present, because Jekyll processing is required.

---

## Development notes

General guidelines for future updates:

- Keep shared page shell code in `_layouts/default.html`.
- Keep shared header/footer/navigation code in `_includes/`.
- Use page front matter to opt into page-specific styles, schema, or lightboxes.
- Prefer root-relative internal links and assets, for example:
  - `/style.css`
  - `/assets/logo.jpg`
  - `/gallery.html`
  - `/events/`
- Keep GTM and GA4 logic centralized through shared includes and GTM.
- Use `dataLayer.push()` for custom interaction tracking.
- Do not duplicate full HTML shell code inside individual pages.

---

## Contributing

This repository primarily serves as the **source of the live site for Kiln Kissed Ceramics**. It is not currently open to outside contributions.

If you spot a typo, broken link, or accessibility issue and would like to help:

- You can open an issue on the repo, or
- Reach out directly through the contact details on the website.

---

## Content & Usage

All copy, design, and images in this repo are for **Kiln Kissed Ceramics**.

- © Kiln Kissed Ceramics by Bhavya Kothari and Bhavya Kothari. All rights reserved.
- Please **do not reuse photos or text** without explicit permission.

If you’d like to feature or share Kiln Kissed Ceramics, please link to the public site rather than copying content directly.
