# Pablitos Auto Sales — Website

Independent used-car dealership website for **Pablitos Auto Sales** in Harbor City, CA.

Live: https://pablitos-auto-sales.pages.dev (current preview)
Production target: https://pablitosautosales.com (after handoff)

## What this is

A single-page static website built in plain HTML, CSS, and vanilla JavaScript. No build step. No framework. Drop the contents of this folder onto any static host (Netlify, Cloudflare Pages, Vercel, S3) and it works.

The inventory grid is **data-driven**: cars live in `data/cars.json`. Edit that file (directly or via the built-in admin UI) and the site updates.

## How inventory works

Each car is one object in `data/cars.json`. The page fetches that file at load time and renders the grid.

Fields per car:

| Field | Type | Notes |
|-------|------|-------|
| `year` | number | e.g. 2014 |
| `make` | string | one of `honda`, `toyota`, `nissan`, `other` |
| `model` | string | e.g. "Camry LE" |
| `miles` | number | mileage, no commas (e.g. 162000) |
| `price` | number | price in dollars, no commas (e.g. 3800) |
| `price_note` | string | optional, e.g. "cash" or "or $200/mo" |
| `transmission` | string | "Automatic" or "Manual" |
| `engine` | string | e.g. "4-cyl", "V6", "7 seats" |
| `image` | string | path to photo, e.g. `/images/civic-2004.jpg` |
| `features` | array | short tag strings (3 recommended). "Clean Title" gets a green badge. |
| `tag` | string | top-left badge text, e.g. "Available", "Sold", "Best Value" |
| `tag_style` | string | "default" (navy) or "deal" (yellow) |

## Two ways to edit inventory

### Option 1 — Decap CMS (no code)

Go to `/admin/` on your live site → log in → click **Inventory** → click **Cars on the Lot** → edit the list. Photos upload directly through the UI; changes commit to git automatically and the site rebuilds in ~30 seconds.

This requires the site to be hosted on **Netlify** with **Netlify Identity** and **git-gateway** enabled. See `HANDOFF.md` for the one-time setup.

### Option 2 — edit the file directly

Open `data/cars.json` in any text editor (or directly on github.com), make changes, save, and either redeploy or push to git. The site reads this file on every page load.

## File map

```
/
├── index.html              # The actual website
├── data/
│   └── cars.json           # Inventory data (edited via /admin/)
├── admin/
│   ├── index.html          # Decap CMS entry point
│   └── config.yml          # CMS schema (defines the form fields)
├── images/
│   ├── hero.jpg            # Lot at golden hour
│   ├── logo.png            # Pablitos bulldog logo
│   ├── og.jpg              # Social-share preview card
│   ├── favicon.png         # Browser tab icon
│   ├── *.jpg               # Car photos
│   └── uploads/            # Where Decap-uploaded photos go
├── netlify.toml            # Netlify build config
└── README.md               # This file
```

## Hosting

Recommended: **Netlify** (free for this traffic level, includes auth for the CMS).

The site is pre-built; Netlify just publishes the root directory. No build command, no install step. Connect this repo to Netlify and it works.

See `HANDOFF.md` for the full setup walkthrough.

## Contact

- Site owner: Frank — (310) 542-2302 — Pablitos Auto Sales
- Built by: Jose @ Juarez Labs (juarezlabs2026@gmail.com)
