# Copilot Instructions — bwhite.id.au Personal Site

## What this project is

A personal "see me" website for Brandon White (VK4BRW). It is **not** a portfolio or professional CV — it's a personal site about hobbies, interests, and cats. Tone is conversational and relaxed.

## Tech stack

- **Jekyll 4.x** static site generator
- **Hydejack v9** theme (free tier), loaded via `remote_theme: hydecorp/hydejack@v9`
- **GitHub Actions** for deployment to **GitHub Pages**
- **Custom domain:** `bwhite.id.au` (CNAME in repo root)
- No JavaScript frameworks, no build pipeline beyond Jekyll

## File structure

```
/                        # repo root
├── _config.yml          # all site config: title, author, theme, nav, social links
├── Gemfile              # Ruby dependencies
├── CNAME                # custom domain: bwhite.id.au
├── index.md             # home page (layout: welcome)
├── about.md             # about page (menu order 1)
├── radio.md             # radio & meshtastic (menu order 2)
├── home-automation.md   # home automation (menu order 3)
├── games.md             # steam games (menu order 4)
├── cats.md              # the cats (menu order 5)
├── find-me.md           # links & contact (menu order 6)
├── assets/
│   └── img/
│       └── avatar.jpg   # 300x300 JPEG sidebar avatar
└── .github/
    └── workflows/
        └── deploy.yml   # GitHub Actions Pages deployment
```

## Content and tone

- **Conversational, first-person, relaxed** — not formal, not CV-like
- Work is mentioned briefly on the About page only ("By day I work in software and systems automation") — do not expand this
- Every page should feel like Brandon talking, not a product description
- The cats are important — treat them with appropriate dignity

## Key personal details

- **Name:** Brandon White
- **Callsign:** VK4BRW (licensed amateur radio operator, SE Queensland)
- **Location:** South-east Queensland, Australia
- **Tagline:** "Building things and breaking things."
- **Email:** brandon@bwhite.id.au
- **GitHub:** bwhite001
- **Steam:** bwhite001
- **Radio club:** Ipswich Amateur Radio Club — https://ipswichradioclub.org.au/

### Hobbies covered on the site
- Ham radio: JOTA, digital modes (FT8/WSPR), WICEN
- Meshtastic: LoRa mesh hobby use
- Home Assistant: presence-based AC, leaving-the-house automations, garden watering
- Steam games: Factorio, Satisfactory, Dyson Sphere Program (automation/factory genre)
- Cats: Wheatley (Burmese cross), Ender (tuxedo), Lucifer (large shorthair), Hex (mini Maine Coon)

## How pages work

Each page is a Markdown file with Jekyll front matter:

```yaml
---
layout: page
title: Page Title
description: One-line SEO description.
menu: true          # adds to Hydejack sidebar nav
order: N            # controls sidebar order (1–6 currently used)
permalink: /slug/   # explicit URL
---
```

The home page uses `layout: welcome` instead of `layout: page`.

## Theme config

All theme settings live in `_config.yml`. Key values:

- `accent_color: "#FFB74D"` — amber, do not change without asking
- `theme_color: "#101010"` — near-black background
- `hydejack.dark_mode.always: true` — dark mode is always on, no toggle
- `author.picture: /assets/img/avatar.jpg` — sidebar avatar

## Deployment

- Push to `master` → GitHub Actions triggers → Jekyll builds → deploys to Pages
- Do not change the default branch from `master`
- The workflow file is `.github/workflows/deploy.yml` — do not modify unless the deployment process itself needs changing

## Conventions

- All page files sit at the repo root (not in subdirectories)
- Image assets go in `assets/img/`
- Avatar must be 300×300px JPEG named `avatar.jpg`
- No blog posts — this site has no `_posts/` directory and no blog functionality
- Keep `docs/plans/` for design and planning documents — do not delete

## What NOT to do

- Do not add a blog or `_posts/` directory
- Do not add a portfolio or projects section
- Do not make the tone professional or CV-like
- Do not change `dark_mode.always` to false — the site is always dark
- Do not commit `avatar.png` (the source file) — only `avatar.jpg` belongs in the repo
- Do not add unnecessary gems or plugins — keep the Gemfile minimal
