# Terminal Theme Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Visually redesign bwhite.id.au with a terminal + circuit-board aesthetic on top of the existing Hydejack v9 free-tier setup, without touching the remote theme.

**Architecture:** Add `_includes/my-head.html` to inject a custom stylesheet, create `assets/css/terminal.css` with all visual overrides, and add `_layouts/terminal-page.html` + `_layouts/welcome.html` as local layouts that Hydejack will prefer over its own. Update all page front matter to use the new layouts.

**Tech Stack:** Jekyll 4.x, Hydejack v9 (remote theme, unchanged), plain CSS (no SCSS pipeline), Google Fonts (JetBrains Mono), inline SVG for circuit ornaments.

---

## Key CSS Selectors (from Hydejack's rendered HTML)

These are the exact selectors to target — verified from the live-built `_site/about/index.html`:

```
body.dark-mode              — the page body
#_sidebar / .sidebar        — sidebar container
.sidebar-bg                 — sidebar background div
.sidebar-sticky             — sticky content wrapper inside sidebar
.sidebar-about              — top-of-sidebar: title + tagline
.sidebar-title              — <a> wrapping the site title
.sidebar-title h2           — the "Brandon White" heading
.sidebar-nav                — nav list container
.sidebar-nav-item           — each nav <a> link
#_navigation                — currently active nav link
#_main                      — main content wrapper (has .layout-page)
article.page                — the page article
h1.page-title               — the page title h1
p.note-sm                   — the description line under the title
.content                    — generic content wrapper
```

---

## Task 1: Create `_includes/my-head.html` (font + CSS injection)

Hydejack includes a local `_includes/my-head.html` file in `<head>` if it exists. This is the correct injection point for custom fonts and CSS without touching the theme.

**Files:**
- Create: `_includes/my-head.html`

**Step 1: Create the file**

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="preload" as="style" href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;600;700&display=swap" onload="this.onload=null;this.rel='stylesheet'">
<noscript><link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;600;700&display=swap"></noscript>
<link rel="stylesheet" href="/assets/css/terminal.css">
```

> Note: The font uses `preload`+`onload` for performance (non-render-blocking), with a `<noscript>` fallback. The terminal.css link loads synchronously since it's our primary stylesheet.

**Step 2: Verify the file exists**

```bash
cat _includes/my-head.html
```
Expected: the 5 lines above, no errors.

**Step 3: Commit**

```bash
git add _includes/my-head.html
git commit -m "feat: add custom head includes for terminal theme font and CSS"
```

---

## Task 2: Create `assets/css/terminal.css` — CSS custom properties + base

This is the main stylesheet. Build it up in stages across Tasks 2–5, then do one final commit per task.

**Files:**
- Create: `assets/css/terminal.css`

**Step 1: Create the file with CSS custom properties and base resets**

```css
/* ============================================================
   Terminal Theme — bwhite.id.au
   Layered on top of Hydejack v9 via custom CSS override.
   ============================================================ */

/* --- Custom properties --- */
:root,
html {
  --term-bg:          #070910;
  --term-surface:     #0d1117;
  --term-text:        #d8dce8;
  --term-muted:       #6b7280;
  --term-amber:       #FFB74D;
  --term-amber-faded: rgba(255, 183, 77, 0.18);
  --term-amber-border:rgba(255, 183, 77, 0.28);
  --term-cyan:        #26C6DA;
  --term-cyan-faded:  rgba(38, 198, 218, 0.25);
  --term-border:      rgba(255, 183, 77, 0.22);

  /* Override Hydejack's own accent variables so its JS hover/focus logic stays consistent */
  --accent-color:           var(--term-amber);
  --accent-color-faded:     var(--term-amber-faded);
  --accent-color-highlight: var(--term-amber-faded);
}

/* --- Base body --- */
body.dark-mode {
  background-color: var(--term-bg) !important;
  color: var(--term-text);
}
```

**Step 2: Verify file was created**

```bash
wc -l assets/css/terminal.css
```
Expected: ~40 lines.

**Step 3: Commit**

```bash
git add assets/css/terminal.css
git commit -m "feat: add terminal.css with custom properties and base body styles"
```

---

## Task 3: Add scanline overlay to `terminal.css`

**Files:**
- Modify: `assets/css/terminal.css` (append)

**Step 1: Append the scanline styles**

```css
/* --- Scanline overlay --- */
body::before {
  content: "";
  pointer-events: none;
  position: fixed;
  inset: 0;
  z-index: 9999;
  background-image: repeating-linear-gradient(
    to bottom,
    transparent,
    transparent 2px,
    rgba(0, 0, 0, 0.04) 2px,
    rgba(0, 0, 0, 0.04) 4px
  );
  mix-blend-mode: multiply;
  opacity: 0.4;
}
```

**Step 2: Commit**

```bash
git add assets/css/terminal.css
git commit -m "feat: add scanline overlay to terminal theme"
```

---

## Task 4: Add sidebar styles to `terminal.css`

**Files:**
- Modify: `assets/css/terminal.css` (append)

**Step 1: Append sidebar styles**

```css
/* ============================================================
   Sidebar
   ============================================================ */

/* Panel */
#_sidebar,
.sidebar {
  background-color: var(--term-surface) !important;
  border-right: 1px solid var(--term-amber) !important;
  box-shadow: 2px 0 16px rgba(0, 0, 0, 0.7) !important;
}

.sidebar-bg {
  background-color: var(--term-surface) !important;
  background-image: none !important;
}

/* Site title */
.sidebar-title {
  text-decoration: none;
}

.sidebar-title h2,
.sidebar-title .h1 {
  font-family: "JetBrains Mono", "Fira Code", "Courier New", monospace;
  color: var(--term-amber) !important;
  font-size: 1.1rem;
  letter-spacing: 0.04em;
  text-shadow: 0 0 12px var(--term-amber-faded);
}

/* Tagline */
.sidebar-about p {
  color: var(--term-muted);
  font-size: 0.78rem;
}

/* Nav links */
.sidebar-nav-item {
  font-family: "JetBrains Mono", "Fira Code", monospace;
  font-size: 0.72rem;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: var(--term-muted) !important;
  transition: color 0.15s ease, text-shadow 0.15s ease;
  padding: 0.3rem 0;
  display: block;
}

.sidebar-nav-item:hover,
#_navigation.sidebar-nav-item {
  color: var(--term-amber) !important;
  text-shadow: 0 0 10px var(--term-amber-faded);
  text-decoration: none;
}

/* Social icons */
.sidebar-social a {
  color: var(--term-muted) !important;
  transition: color 0.15s ease;
}

.sidebar-social a:hover {
  color: var(--term-cyan) !important;
}

/* Avatar — amber ring + cyan status LED */
.sidebar-about img {
  border-radius: 50%;
  box-shadow: 0 0 0 2px var(--term-amber), 0 0 16px var(--term-amber-faded);
  position: relative;
}
```

**Step 2: Commit**

```bash
git add assets/css/terminal.css
git commit -m "feat: add sidebar styles to terminal theme"
```

---

## Task 5: Add content/typography styles to `terminal.css`

**Files:**
- Modify: `assets/css/terminal.css` (append)

**Step 1: Append typography and content styles**

```css
/* ============================================================
   Typography & Content
   ============================================================ */

/* Headings — monospace */
h1, h2, h3,
.page-title {
  font-family: "JetBrains Mono", "Fira Code", "Courier New", monospace;
  letter-spacing: 0.02em;
}

/* h1 terminal prompt prefix */
h1::before,
.page-title::before {
  content: "> ";
  color: var(--term-amber);
  opacity: 0.8;
}

/* Content column width — ~80-col terminal feel */
#_main .content,
article.page {
  max-width: 72ch;
}

/* Links */
a {
  color: var(--term-amber);
}

a:hover {
  color: var(--term-amber);
  text-shadow: 0 0 8px var(--term-amber-faded);
}

/* Page title */
.page-title {
  color: var(--term-text);
}

/* Description under title */
p.note-sm {
  color: var(--term-muted);
  font-size: 0.85rem;
  border-left: 2px solid var(--term-cyan-faded);
  padding-left: 0.75rem;
  margin-bottom: 1.5rem;
}

/* Code blocks */
pre, code {
  font-family: "JetBrains Mono", "Fira Code", "Courier New", monospace !important;
  background-color: var(--term-surface) !important;
}

pre {
  border-left: 3px solid var(--term-cyan) !important;
  border-radius: 0 4px 4px 0;
}

/* Horizontal rules */
hr {
  border-color: var(--term-border);
}
```

**Step 2: Commit**

```bash
git add assets/css/terminal.css
git commit -m "feat: add typography and content styles to terminal theme"
```

---

## Task 6: Add `.pcb-frame` circuit frame styles to `terminal.css`

The `.pcb-frame` class is applied by the custom layouts (Tasks 7–8). These styles give the circuit-board border effect.

**Files:**
- Modify: `assets/css/terminal.css` (append)

**Step 1: Append pcb-frame styles**

```css
/* ============================================================
   Circuit Frame (.pcb-frame)
   Applied by terminal-page.html and welcome.html layouts
   ============================================================ */

.pcb-frame {
  position: relative;
  border-left: 2px solid rgba(38, 198, 218, 0.3);
  padding-left: 1.5rem;
  padding-top: 0.5rem;
}

/* SVG corner ornaments are inlined in the layouts */
.pcb-corner {
  position: absolute;
  pointer-events: none;
  opacity: 0.7;
}

.pcb-corner--tr {
  top: 0;
  right: 0;
}

.pcb-corner--bl {
  bottom: 0;
  left: 0;
  transform: rotate(180deg);
}
```

**Step 2: Commit**

```bash
git add assets/css/terminal.css
git commit -m "feat: add pcb-frame circuit styles to terminal theme"
```

---

## Task 7: Create `_layouts/terminal-page.html`

This layout extends Hydejack's `page` layout, wrapping `{{ content }}` in `.pcb-frame` with two inline SVG circuit corner ornaments.

**Files:**
- Create: `_layouts/terminal-page.html`

**Step 1: Create the file**

```html
---
layout: page
---

<div class="pcb-frame">

  <!-- Top-right circuit corner (amber) -->
  <svg class="pcb-corner pcb-corner--tr" width="60" height="60" viewBox="0 0 60 60"
       fill="none" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
    <path d="M58 2 H30 Q20 2 20 12 V58" stroke="#FFB74D" stroke-width="1.5"/>
    <circle cx="20" cy="58" r="3" fill="#FFB74D"/>
    <circle cx="58" cy="2" r="2.5" fill="#FFB74D" opacity="0.5"/>
  </svg>

  <!-- Bottom-left circuit corner (cyan) -->
  <svg class="pcb-corner pcb-corner--bl" width="60" height="60" viewBox="0 0 60 60"
       fill="none" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
    <path d="M58 2 H30 Q20 2 20 12 V58" stroke="#26C6DA" stroke-width="1.5"/>
    <circle cx="20" cy="58" r="3" fill="#26C6DA"/>
    <circle cx="58" cy="2" r="2.5" fill="#26C6DA" opacity="0.5"/>
  </svg>

  {{ content }}

</div>
```

**Step 2: Verify the file**

```bash
cat _layouts/terminal-page.html
```
Expected: the file with `layout: page` front matter and the two SVGs.

**Step 3: Commit**

```bash
git add _layouts/terminal-page.html
git commit -m "feat: add terminal-page layout with circuit corner ornaments"
```

---

## Task 8: Create `_layouts/welcome.html`

The home page layout. Extends Hydejack's `page` layout (which provides the full chrome), wraps content in `.pcb-frame`. No typed-cursor effect — just the standard terminal look.

**Files:**
- Create: `_layouts/welcome.html`

**Step 1: Create the file**

```html
---
layout: page
---

<div class="pcb-frame">

  <!-- Top-right circuit corner (amber) -->
  <svg class="pcb-corner pcb-corner--tr" width="60" height="60" viewBox="0 0 60 60"
       fill="none" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
    <path d="M58 2 H30 Q20 2 20 12 V58" stroke="#FFB74D" stroke-width="1.5"/>
    <circle cx="20" cy="58" r="3" fill="#FFB74D"/>
    <circle cx="58" cy="2" r="2.5" fill="#FFB74D" opacity="0.5"/>
  </svg>

  <!-- Bottom-left circuit corner (cyan) -->
  <svg class="pcb-corner pcb-corner--bl" width="60" height="60" viewBox="0 0 60 60"
       fill="none" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
    <path d="M58 2 H30 Q20 2 20 12 V58" stroke="#26C6DA" stroke-width="1.5"/>
    <circle cx="20" cy="58" r="3" fill="#26C6DA"/>
    <circle cx="58" cy="2" r="2.5" fill="#26C6DA" opacity="0.5"/>
  </svg>

  {{ content }}

</div>
```

**Step 2: Verify the file**

```bash
cat _layouts/welcome.html
```

**Step 3: Update `index.md` to use this layout**

Change `index.md` front matter:
```yaml
---
layout: welcome
title: Hi, I'm Brandon
cover: false
---
```

(It currently uses `layout: home` following the recent fix — change it back to `welcome` now that we have a local `welcome.html`.)

**Step 4: Commit**

```bash
git add _layouts/welcome.html index.md
git commit -m "feat: add welcome layout for home page; restore layout: welcome in index.md"
```

---

## Task 9: Update all page front matter to `layout: terminal-page`

**Files:**
- Modify: `about.md` (line 2)
- Modify: `radio.md` (line 2)
- Modify: `home-automation.md` (line 2)
- Modify: `games.md` (line 2)
- Modify: `cats.md` (line 2)
- Modify: `find-me.md` (line 2)

**Step 1: Update all six files**

In each file, change:
```yaml
layout: page
```
to:
```yaml
layout: terminal-page
```

**Step 2: Verify all six files changed**

```bash
grep -n "^layout:" about.md radio.md home-automation.md games.md cats.md find-me.md
```
Expected: all six show `layout: terminal-page`

**Step 3: Commit**

```bash
git add about.md radio.md home-automation.md games.md cats.md find-me.md
git commit -m "feat: switch all pages to terminal-page layout"
```

---

## Task 10: Local build verification

**Step 1: Install dependencies (if needed)**

```bash
bundle install
```

**Step 2: Build the site**

```bash
bundle exec jekyll build 2>&1
```
Expected: `done in X seconds` with no errors. Sass deprecation warnings from Hydejack are expected and fine.

**Step 3: Check that layouts were applied**

```bash
head -5 _site/index.html
grep -c "pcb-frame" _site/about/index.html
grep -c "pcb-frame" _site/index.html
```
Expected:
- `_site/index.html` starts with `<!DOCTYPE html>` (not bare `<p>` tags)
- Both greps return `1` or more

**Step 4: Check the custom CSS is linked**

```bash
grep "terminal.css" _site/about/index.html
```
Expected: a `<link rel="stylesheet" href="/assets/css/terminal.css">` line.

**Step 5: Check font is linked**

```bash
grep "JetBrains" _site/about/index.html
```
Expected: the Google Fonts link containing `JetBrains+Mono`.

**Step 6: Push to deploy**

```bash
git push
```

Expected: GitHub Actions CI run succeeds, site deploys to https://www.bwhite.id.au/ with the new terminal look.

---

## Out of Scope (future follow-up)

- Per-page colour accents (radio = signal dots, games = grid background)
- Animated typed-cursor on home page
- Avatar picture status LED (requires inspect of avatar HTML — may need a wrapper `<div>` in a custom sidebar include)
- Custom 404 page with terminal error aesthetic
