# Personal "See Me" Site — Design Document

## Problem & Goal

Build a personal "about me / see me" website at `bwhite.id.au`, hosted on GitHub Pages.
This is **not** a portfolio or professional advertisement — it's a quiet corner of the internet
that feels like Brandon: his hobbies, interests, cats, and where to find him online.

## Approach

Multi-page Jekyll site using the **Hydejack** theme (free tier).

Chosen because:
- Sidebar navigation is built in — perfect for 7 standalone pages
- Dark mode available out of the box (since v9.2)
- Designed specifically for personal "see me" sites
- GitHub Pages compatible
- Polished and modern without being over-engineered

## Site Structure

| Page            | URL                 | Purpose                                              |
|-----------------|---------------------|------------------------------------------------------|
| Home            | `/`                 | Short intro, personality, cat mention, nav cards     |
| About           | `/about`            | Who I am, light work mention, values                 |
| Radio & Mesh    | `/radio`            | Ham radio VK4BRW + Meshtastic                        |
| Home Automation | `/home-automation`  | Home Assistant automations                           |
| Games           | `/games`            | Automation/factory games on Steam                    |
| Cats            | `/cats`             | The four cats                                        |
| Find Me         | `/find-me`          | All external links                                   |

## File Layout

```
me-page/
├── _config.yml           # Hydejack config, title, author, nav, social links
├── Gemfile               # jekyll + hydejack gem
├── index.md              # Home page
├── about.md              # About page
├── radio.md              # Radio & Meshtastic
├── home-automation.md    # Home Automation
├── games.md              # Games
├── cats.md               # Cats
├── find-me.md            # Find Me / links
├── assets/
│   └── img/
│       └── sidebar-bg.jpg    # Optional sidebar background image
└── docs/
    └── plans/
        └── 2026-03-01-personal-site-design.md
```

## Content Spec

### Home (`index.md`)
- 1–2 sentence intro: Brandon, Queensland, tinkerer
- Brief mention of the four cats
- Link cards / list to each section

### About (`about.md`)
- "Who I am" paragraph — QLD, builds and automates things
- Short work paragraph: "By day I work in software and automation" — not the focus
- Values / personality: learning, community, making things work quietly

### Radio & Meshtastic (`radio.md`)
- **Ham radio — VK4BRW**
  - Activities: JOTA, digital modes, WICEN (helping people get on air)
  - Links: QRZ profile, Ipswich Amateur Radio Club (https://ipswichradioclub.org.au/)
- **Meshtastic**
  - Hobby use: mesh comms, off-grid tinkering, coverage experiments

### Home Automation (`home-automation.md`)
- Platform: Home Assistant
- Featured automations:
  - AC control based on presence/temperature
  - Scheduling tasks when people leave the house (garden watering, etc.)
- Tone: ongoing tinkering, house that "just does the right thing"

### Games (`games.md`)
- Steam handle: `bwhite001`
- Genre: automation / factory games
- Titles: Factorio, Satisfactory, Dyson Sphere Program (and others from the genre)
- Style: build it → destroy it → replay it; relaxing systems thinking

### Cats (`cats.md`)
- **Wheatley** — Burmese cross
- **Ender** — tuxedo cat
- **Lucifer** — large shorthair mix
- **Hex** — mini Maine Coon (the "sassy" one)
- Short blurb per cat, conversational tone

### Find Me (`find-me.md`)
- Email: `bwhite.id.au` domain
- GitHub: https://github.com/bwhite001
- Steam: https://steamcommunity.com/id/bwhite001
- QRZ: (VK4BRW profile)
- Ipswich Amateur Radio Club: https://ipswichradioclub.org.au/

## Visual Design

- **Theme:** Hydejack (free, v9.x)
- **Mode:** Dark (enabled in `_config.yml`)
- **Sidebar:** Avatar placeholder + "Brandon White" + tagline "Building things and breaking things." + nav links
- **Accent colour:** Amber `#FFB74D` (warm, ham-radio-ish)
- **Tone:** Friendly, minimal, personal — not a dev portfolio

## Decisions Made

| Decision | Choice | Reason |
|----------|--------|--------|
| Stack | Jekyll + Hydejack | GitHub Pages native, dark sidebar built-in |
| Layout | Multi-page (Option C) | Each topic deserves its own space |
| Career | Short paragraph on About only | Context without advertising |
| Cats | Own page | They deserve it |
| Visual | Dark theme | Techy/moody, Brandon's preference |
