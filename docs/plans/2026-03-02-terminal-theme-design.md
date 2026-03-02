# Terminal Theme Design — bwhite.id.au

**Date:** 2026-03-02  
**Status:** Approved

---

## Problem & Goal

The site currently uses Hydejack v9 (free tier) via `remote_theme`, which gives a clean but stock look. The goal is a full visual redesign to a **terminal + circuit-board aesthetic** — dark, technical, personal — while keeping Hydejack's structural plumbing (sidebar nav, Jekyll processing, SEO, dark mode) in place.

---

## Approach: CSS Override Layer + Custom Layouts

Keep `remote_theme: hydecorp/hydejack@v9` unchanged. Add:

- Custom layouts that shadow Hydejack's (`_layouts/` files in the repo take precedence over the theme)
- A single custom stylesheet injected via Hydejack's `custom_css` config

No Gemfile changes. No build pipeline. No Webpack. Low maintenance.

---

## New Files

```
_layouts/
  welcome.html          — home page layout (fixes missing PRO layout; extends page)
  terminal-page.html    — terminal-themed wrapper for all other pages (extends page)

assets/css/
  terminal.css          — all visual overrides (colours, type, sidebar, circuit frames)
```

### `_layouts/welcome.html`

Chains into Hydejack's `page` layout. Adds:
- `pcb-frame` div wrapper around `{{ content }}`
- No typed-cursor JS — just the amber `>_` prefix on headings via CSS `::before`

### `_layouts/terminal-page.html`

Chains into Hydejack's `page` layout. Adds:
- `pcb-frame` div wrapper around `{{ content }}`
- Two inline SVGs for circuit-trace corner ornaments (top-right, bottom-left) in amber + cyan

### `_config.yml` change

```yaml
hydejack:
  custom_css:
    - /assets/css/terminal.css
```

---

## Page Front Matter Changes

All pages switch layout:

| File | Current layout | New layout |
|------|---------------|------------|
| `index.md` | `home` (was `welcome`) | `welcome` |
| `about.md` | `page` | `terminal-page` |
| `radio.md` | `page` | `terminal-page` |
| `home-automation.md` | `page` | `terminal-page` |
| `games.md` | `page` | `terminal-page` |
| `cats.md` | `page` | `terminal-page` |
| `find-me.md` | `page` | `terminal-page` |

---

## Colour System

| Token | Value | Use |
|---|---|---|
| `--bg` | `#070910` | Page + sidebar background |
| `--text` | `#d8dce8` | Body copy |
| `--text-muted` | `#6b7280` | Meta, captions, timestamps |
| `--amber` | `#FFB74D` | Primary accent — links, active nav, h1–h3 |
| `--amber-glow` | `rgba(255,183,77,0.15)` | Hover halos, soft borders |
| `--cyan` | `#26C6DA` | Secondary accent — status LED, circuit traces, hover states |
| `--surface` | `#0d1117` | Sidebar panel, code block background |
| `--border` | `rgba(255,183,77,0.25)` | Hairline amber borders |

Hydejack CSS custom properties (`--accent-color`, `--body-bg`, `--body-color`) are overridden at `:root` so the Hydejack JS dark-mode and hover logic still picks up the right values.

---

## Typography

- **Headings (h1–h3), nav, sidebar author name:** `JetBrains Mono` (Google Fonts), fallback `"Fira Code", "Courier New", monospace`
- **Body:** keep Hydejack default (`-apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif`) — readability over aesthetics for body text
- **h1 `::before`:** `"> "` prefix in amber — terminal prompt feel
- **Content column:** `max-width: 72ch` — narrower than Hydejack's default, 80-column terminal feel
- **Links:** amber, underline on hover, subtle amber glow on focus

---

## Scanline Overlay

```css
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
    rgba(0,0,0,0.03) 2px,
    rgba(0,0,0,0.03) 4px
  );
  mix-blend-mode: multiply;
  opacity: 0.4;
}
```

Subtle at 40% opacity + multiply blend — adds texture on OLED-dark backgrounds without affecting readability.

---

## Sidebar

- Background: `var(--surface)` (`#0d1117`)
- Right border: `1px solid var(--amber)` + `box-shadow: 2px 0 12px rgba(0,0,0,0.6)`
- **Avatar:** circular, `2px solid var(--amber)` ring, cyan "online" status LED pseudo-element bottom-right (`8px` circle, `box-shadow: 0 0 8px var(--cyan)`)
- **Author name:** `var(--amber)`, monospace, slight letter-spacing
- **Nav links:** monospace uppercase `0.75rem`, muted default (`#6b7280`), active/hover → `var(--amber)` + `text-shadow: 0 0 8px var(--amber-glow)`
- **Social icons:** default muted, hover → cyan

---

## Circuit Frame (`.pcb-frame`)

Applied to the `<main>` content wrapper in both layouts:

- `border-left: 2px solid rgba(38,198,218,0.3)` — subtle cyan left rail
- `padding-left: 1.5rem`
- Two absolutely-positioned inline SVGs (60×60px, `pointer-events: none`):
  - **Top-right:** amber `L`-shaped trace with 3px node dot at corner
  - **Bottom-left:** cyan `L`-shaped trace with 3px node dot at corner

SVG markup is inlined in the layout templates (no external file dependency).

---

## Code Blocks

```css
pre, code {
  background: var(--surface);
  border-left: 3px solid var(--cyan);
  font-family: "JetBrains Mono", monospace;
}
```

---

## Out of Scope

- Per-page colour variations (radio = signal dots, games = grid, etc.) — can be added later as a follow-up
- Animated typed-cursor on home page — deferred; adds JS complexity for minimal gain
- Dark/light mode toggle — Hydejack dark mode stays always-on as currently configured
- Any changes to the content of existing pages
