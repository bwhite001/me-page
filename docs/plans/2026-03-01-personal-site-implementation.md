# Personal "See Me" Site — Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build a personal multi-page "see me" Jekyll site using Hydejack theme, deployed to GitHub Pages at `bwhite.id.au`.

**Architecture:** Multi-page Jekyll site with Hydejack theme (free tier). 7 standalone pages (Home, About, Radio, Home Automation, Games, Cats, Find Me) with a sidebar nav and dark mode. No blog, no portfolio — just personal pages.

**Tech Stack:** Ruby/Jekyll, Hydejack gem (jekyll-theme-hydejack), GitHub Pages (via `github-pages` gem or direct Jekyll deployment with Actions).

---

## Context: Hydejack Setup Notes

- Hydejack free tier: install via `gem "jekyll-theme-hydejack"` in Gemfile
- Dark mode: set `theme_color: '#101010'` and `accent_color: '#4FB3BF'` (teal) in `_config.yml`, plus `hydejack.dark_mode.always: true`
- Sidebar author info is driven by `author:` key in `_config.yml`
- Nav pages appear in sidebar via `menu: true` in each page's front matter
- Social links in sidebar via `author.social:` array in `_config.yml`
- GitHub Pages: use `remote_theme: hydecorp/hydejack@v9` with `jekyll-remote-theme` plugin for Pages compatibility

---

## Task 1: Scaffold the Jekyll project

**Files:**
- Create: `Gemfile`
- Create: `_config.yml`
- Create: `.gitignore`

**Step 1: Create the Gemfile**

```ruby
# Gemfile
source "https://rubygems.org"

gem "jekyll", "~> 4.3"
gem "jekyll-theme-hydejack", "~> 9.2"
gem "jekyll-remote-theme"
gem "webrick", "~> 1.8"

group :jekyll_plugins do
  gem "jekyll-feed"
  gem "jekyll-sitemap"
  gem "jekyll-seo-tag"
end
```

**Step 2: Create `_config.yml`**

```yaml
# _config.yml
title: Brandon White
tagline: VK4BRW · QLD · Tinkerer
description: >-
  A small corner of the internet that's just about me —
  ham radio, home automation, cats, and other things I enjoy.

url: "https://bwhite.id.au"
baseurl: ""

remote_theme: hydecorp/hydejack@v9

# Hydejack settings
hydejack:
  dark_mode:
    always: true
    dynamic: false
    icon: false
  accent_image:
    background: "#1a1a2e"
    overlay: false

accent_color: "#4FB3BF"
theme_color: "#101010"

# Sidebar author card
author:
  name: Brandon White
  picture: /assets/img/avatar.jpg
  email: brandon@bwhite.id.au
  social:
    - title: GitHub
      url: https://github.com/bwhite001
      icon: github
    - title: Steam
      url: https://steamcommunity.com/id/bwhite001
      icon: steam
    - title: Ipswich Radio Club
      url: https://ipswichradioclub.org.au/
      icon: radio

# Plugins
plugins:
  - jekyll-feed
  - jekyll-sitemap
  - jekyll-seo-tag
  - jekyll-remote-theme

# Build settings
markdown: kramdown
highlighter: rouge
permalink: /:title/

exclude:
  - Gemfile
  - Gemfile.lock
  - docs/
  - node_modules/
  - vendor/
```

**Step 3: Create `.gitignore`**

```
_site/
.jekyll-cache/
.jekyll-metadata
.bundle/
vendor/
Gemfile.lock
```

**Step 4: Install gems**

```bash
cd /brandon/me-page
bundle install
```

Expected: Gems install without errors. Hydejack and its dependencies resolve.

**Step 5: Verify Jekyll builds**

```bash
bundle exec jekyll build 2>&1 | tail -20
```

Expected: Build completes (may warn about missing pages — that's fine at this stage).

**Step 6: Commit**

```bash
git add Gemfile _config.yml .gitignore
git commit -m "feat: scaffold Jekyll project with Hydejack theme

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>"
```

---

## Task 2: Home page (`index.md`)

**Files:**
- Create: `index.md`

**Step 1: Create `index.md`**

```markdown
---
layout: welcome
title: Hi, I'm Brandon
cover: false
---

G'day — I'm Brandon, living in south-east Queensland, Australia.

This is a small corner of the internet that's just about me: the things I
tinker with, the hobbies I get lost in, and the four cats who rule the house.

---

## What's here

- [About me](/about/) — who I am, in a paragraph or two
- [Radio & Meshtastic](/radio/) — ham radio as VK4BRW, and mesh comms
- [Home automation](/home-automation/) — Home Assistant and making the house smart
- [Games](/games/) — automation and factory games on Steam
- [The cats](/cats/) — Wheatley, Ender, Lucifer, and Hex
- [Find me](/find-me/) — where I hang out online
```

**Step 2: Serve locally and check**

```bash
bundle exec jekyll serve --livereload
```

Open http://localhost:4000 — confirm the home page renders with Hydejack sidebar and nav.

**Step 3: Commit**

```bash
git add index.md
git commit -m "feat: add home page

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>"
```

---

## Task 3: About page (`about.md`)

**Files:**
- Create: `about.md`

**Step 1: Create `about.md`**

```markdown
---
layout: page
title: About
description: >-
  Who I am — Queensland, tinkering, and a few things I care about.
menu: true
order: 1
---

I'm Brandon. I live in south-east Queensland, Australia, and I enjoy building
and automating things — whether that's software, radio gear, or bits of the house.

By day I work in software and systems automation. It's not why this site exists,
but it does explain why I find it hard to leave a thing un-automated.

Outside of work, I tend to disappear into whichever technical rabbit hole is
currently interesting — right now that means ham radio, Home Assistant, and a
mesh radio network in the backyard. When I need a break from all of that, I
build factories in video games, which feels about right.

I value learning things properly, helping out in communities I'm part of, and
making things work quietly in the background so I don't have to think about them.

Four cats also live here. They have opinions.
```

**Step 2: Check sidebar nav**

Serve locally (`bundle exec jekyll serve`) and confirm "About" appears in the sidebar nav.

**Step 3: Commit**

```bash
git add about.md
git commit -m "feat: add about page

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>"
```

---

## Task 4: Radio & Meshtastic page (`radio.md`)

**Files:**
- Create: `radio.md`

**Step 1: Create `radio.md`**

```markdown
---
layout: page
title: Radio & Meshtastic
description: >-
  Ham radio as VK4BRW, JOTA, digital modes, WICEN, and Meshtastic mesh comms.
menu: true
order: 2
---

## Ham radio — VK4BRW

I'm a licensed amateur radio operator based in south-east Queensland,
operating as **VK4BRW**.

Radio for me is as much about community as it is about the tech. I enjoy:

- **JOTA** (Jamboree on the Air) — connecting Scouts around the world via radio
- **Digital modes** — FT8, WSPR, and other weak-signal modes where the software
  does the heavy lifting
- **WICEN** — helping provide communications support at community events, and
  helping newcomers get on the air

I'm a member of the [Ipswich Amateur Radio Club](https://ipswichradioclub.org.au/),
which is a great community for anyone in the region interested in getting licensed
or exploring the hobby.

---

## Meshtastic

Meshtastic is a low-power, long-range mesh radio network running on cheap
LoRa hardware. No internet required — nodes talk directly to each other.

I use it as a hobby project: experimenting with coverage, linking nodes,
and seeing how far a small device can reach. It scratches the same itch as
ham radio — off-grid comms, tinkering with hardware, and understanding how
signals propagate — but at a much lower barrier to entry.

If you're in the area and running a node, feel free to say hello.
```

**Step 2: Verify and commit**

```bash
git add radio.md
git commit -m "feat: add radio & meshtastic page

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>"
```

---

## Task 5: Home Automation page (`home-automation.md`)

**Files:**
- Create: `home-automation.md`

**Step 1: Create `home-automation.md`**

```markdown
---
layout: page
title: Home Automation
description: >-
  Home Assistant, presence-based AC, garden watering, and making the house
  quietly do the right thing.
menu: true
order: 3
---

I run [Home Assistant](https://www.home-assistant.io/) for most of my home
automation. The appeal for me is wiring together sensors, presence, and schedules
so that the house just quietly does the right thing without me thinking about it.

A few things I've set up that I actually use every day:

**Presence-based AC**
The air conditioning adjusts automatically based on which room I'm in,
the time of day, and the temperature outside. It's one of those things
that, once it works, you forget you set it up — which is exactly the goal.

**Leaving-the-house automations**
When everyone leaves, a handful of things kick off: the garden gets watered
on schedule, lights and devices that were left on get tidied up, and the
house goes into a low-power idle state. It's a surprisingly satisfying set
of automations to build.

Home Assistant is one of those rabbit holes that's very easy to fall into.
I keep adding bits and pieces, and it keeps getting more useful.
```

**Step 2: Verify and commit**

```bash
git add home-automation.md
git commit -m "feat: add home automation page

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>"
```

---

## Task 6: Games page (`games.md`)

**Files:**
- Create: `games.md`

**Step 1: Create `games.md`**

```markdown
---
layout: page
title: Games
description: >-
  Automation and factory games on Steam — build it, destroy it, replay it.
menu: true
order: 4
---

When I have downtime, I end up in automation and factory games on Steam
as **[bwhite001](https://steamcommunity.com/id/bwhite001)**.

The genre is a good fit: you build systems, tune bottlenecks, watch
everything click into place — and then start over and do it better.
There's something genuinely relaxing about optimising a production line
at midnight.

Games I keep coming back to:

- **Factorio** — the one that started it. Build a factory. Launch a rocket.
  Realise you could have done it more efficiently. Do it again.
- **Satisfactory** — same idea, first person, prettier. Very good for
  when you want to spend an hour just building infrastructure.
- **Dyson Sphere Program** — build a factory that spans an entire solar
  system. Extremely ambitious. Also extremely good.

The pattern is the same across all of them: build something that works,
tear it down when you understand it better, rebuild it cleaner.
That loop never really gets old.
```

**Step 2: Verify and commit**

```bash
git add games.md
git commit -m "feat: add games page

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>"
```

---

## Task 7: Cats page (`cats.md`)

**Files:**
- Create: `cats.md`

**Step 1: Create `cats.md`**

```markdown
---
layout: page
title: The Cats
description: >-
  Wheatley, Ender, Lucifer, and Hex — the four cats who actually run this house.
menu: true
order: 5
---

Four cats live here. They have strong opinions about most things.

---

**Wheatley** — Burmese cross

Named after the AI from Portal 2, mostly because he was a bit of a disaster
as a kitten and has never fully recovered his reputation. Friendly, loud,
and convinced that he deserves more food than the others.

---

**Ender** — Tuxedo cat

Classic tuxedo markings. Named after Ender Wiggin. Very dignified.
Will sit on whatever you're currently using and make eye contact until
you acknowledge him.

---

**Lucifer** — Large shorthair mix

Large. Very large. Has a name that does not match his personality at all —
he's gentle, patient, and mostly just wants a warm spot to sleep.

---

**Hex** — Mini Maine Coon

The smallest one and absolutely aware of it. Compensates with an outsized
personality and a very loud meow for her size. Sassy is an understatement.
```

**Step 2: Verify and commit**

```bash
git add cats.md
git commit -m "feat: add cats page

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>"
```

---

## Task 8: Find Me page (`find-me.md`)

**Files:**
- Create: `find-me.md`

**Step 1: Create `find-me.md`**

```markdown
---
layout: page
title: Find Me
description: >-
  Where to find Brandon online — GitHub, Steam, QRZ, email, and the radio club.
menu: true
order: 6
---

A few places I'm actually active online:

**GitHub**
[github.com/bwhite001](https://github.com/bwhite001) — mostly personal tools,
automation scripts, and bits of tinkering.

**Steam**
[bwhite001](https://steamcommunity.com/id/bwhite001) — automation and factory
games, mostly. Don't expect a chat response mid-Factorio session.

**Ham radio — VK4BRW**
Find me on [QRZ](https://www.qrz.com/db/VK4BRW) or come say hello on the
[Ipswich Amateur Radio Club](https://ipswichradioclub.org.au/) repeaters.

**Email**
[brandon@bwhite.id.au](mailto:brandon@bwhite.id.au) — I do read it, I'm just
slow to reply.
```

**Step 2: Verify and commit**

```bash
git add find-me.md
git commit -m "feat: add find me page

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>"
```

---

## Task 9: GitHub Pages deployment

**Files:**
- Create: `.github/workflows/deploy.yml`

**Step 1: Create the GitHub Actions workflow**

```yaml
# .github/workflows/deploy.yml
name: Deploy Jekyll site to GitHub Pages

on:
  push:
    branches: ["master", "main"]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Setup Ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: '3.1'
          bundler-cache: true
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v4
      - name: Build with Jekyll
        run: bundle exec jekyll build --baseurl "${{ steps.pages.outputs.base_path }}"
        env:
          JEKYLL_ENV: production
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

**Step 2: Configure GitHub repo for Pages**

In your GitHub repo settings:
1. Settings → Pages → Source → **GitHub Actions**
2. Ensure the repo is public (or you have Pages on private repos)

**Step 3: Add a custom domain (optional)**

Create a `CNAME` file in the repo root:

```
bwhite.id.au
```

Then in your DNS provider, add:
- `A` records pointing to GitHub Pages IPs:
  - `185.199.108.153`
  - `185.199.109.153`
  - `185.199.110.153`
  - `185.199.111.153`
- Or a `CNAME` record pointing `bwhite.id.au` → `<your-github-username>.github.io`

**Step 4: Commit**

```bash
git add .github/ CNAME
git commit -m "feat: add GitHub Pages deployment workflow

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>"
```

---

## Task 10: Avatar placeholder

**Files:**
- Create: `assets/img/` directory

**Step 1: Create the assets directory and add a placeholder note**

```bash
mkdir -p assets/img
```

Add a placeholder text file until a real avatar is available:

```bash
echo "# Add avatar.jpg here (recommended: 300x300px square, JPG or PNG)" > assets/img/README.md
```

Update `_config.yml` if no avatar exists yet — comment out the `picture:` line or point to a placeholder:

In `_config.yml`, change:
```yaml
  picture: /assets/img/avatar.jpg
```
to:
```yaml
  # picture: /assets/img/avatar.jpg  # add avatar.jpg when ready
```

**Step 2: Commit**

```bash
git add assets/
git commit -m "chore: add assets/img directory for avatar

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>"
```

---

## Task 11: Final local review

**Step 1: Full local build and serve**

```bash
bundle exec jekyll serve --livereload
```

Open http://localhost:4000 and check:
- [ ] Sidebar shows: name, tagline, nav links (About, Radio, Home Automation, Games, Cats, Find Me)
- [ ] Dark mode is active
- [ ] All 7 pages render without errors
- [ ] All internal links work
- [ ] Social links in sidebar/footer are correct

**Step 2: Fix any issues found**

**Step 3: Final commit and push**

```bash
git push origin master
```

Watch the GitHub Actions workflow complete and verify the site is live at the Pages URL.

---

## Notes for the implementer

- **Hydejack accent colour:** `#4FB3BF` (teal) is suggested. Change `accent_color` in `_config.yml` if you want a different hue.
- **Avatar:** Drop a 300×300px JPG/PNG as `assets/img/avatar.jpg` when ready. No placeholder image is needed — Hydejack renders a placeholder letter-avatar if the file is missing.
- **QRZ URL:** The plan uses `https://www.qrz.com/db/VK4BRW` — verify this is the correct direct link before pushing.
- **Email:** `brandon@bwhite.id.au` is used throughout. Confirm this is the address you want public.
- **Steam URL:** `https://steamcommunity.com/id/bwhite001` — verify the vanity URL is correct for your account.
- **Cats:** The personality blurbs are written to match the names/breeds provided. Edit freely to taste.
