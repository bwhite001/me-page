# bwhite.id.au — Personal Site

Personal "see me" site for Brandon White (VK4BRW), hosted on GitHub Pages at [bwhite.id.au](https://bwhite.id.au).

Built with [Jekyll](https://jekyllrb.com/) and the [Hydejack](https://hydejack.com/) theme (v9, free tier).

---

## Pages

| Page | URL | Content |
|------|-----|---------|
| Home | `/` | Intro and nav |
| About | `/about/` | Who I am |
| Radio & Meshtastic | `/radio/` | VK4BRW, JOTA, digital modes, WICEN, Meshtastic |
| Home Automation | `/home-automation/` | Home Assistant automations |
| Games | `/games/` | Automation/factory games on Steam |
| The Cats | `/cats/` | Wheatley, Ender, Lucifer, Hex |
| Find Me | `/find-me/` | Links and contact |

---

## Local development

**Prerequisites:** Ruby 3.x and Bundler.

```bash
bundle install
bundle exec jekyll serve --livereload
```

Open [http://localhost:4000](http://localhost:4000).

### Using Docker (no Ruby install needed)

```bash
docker run --rm -v "$PWD":/site -w /site -p 4000:4000 ruby:3.2 \
  bash -c "gem install bundler && bundle install && bundle exec jekyll serve --host 0.0.0.0"
```

---

## Deployment

Pushes to `master` automatically deploy via GitHub Actions to GitHub Pages.

Workflow: [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml)

Custom domain is set in `CNAME` → `bwhite.id.au`.

---

## Adding or editing content

All pages are plain Markdown files at the repo root. Edit the file, commit, and push — the site redeploys automatically.

**To add a new page:**

1. Create a new `.md` file at the repo root
2. Add this front matter:

```yaml
---
layout: page
title: Your Page Title
menu: true
order: 7          # controls sidebar position
permalink: /your-page/
---
```

3. Add your content below the front matter

**To update the sidebar:**
- Name, tagline, accent colour, social links: edit `_config.yml`

**To add an avatar:**
- Drop `avatar.jpg` (300×300px) into `assets/img/`
- Uncomment `picture:` in `_config.yml`

---

## Theme

[Hydejack v9](https://hydejack.com/) free tier. Dark mode forced on. Amber accent (`#FFB74D`).

Key config in `_config.yml`:

```yaml
accent_color: "#FFB74D"
theme_color: "#101010"
hydejack:
  dark_mode:
    always: true
```

---

## DNS

`bwhite.id.au` points to GitHub Pages via `A` records:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```
