# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static HTML/CSS/JS website for the "Accept the Cookies" arts collective. Vanilla — **no build step, no package manager, no JS framework**. The repository root is the publish directory.

`AGENTS.md` is the canonical source of truth for project conventions (styling rules, layout constraints, content preservation rules). Read it before making changes.

## Common Commands

**Local preview** (no build needed):
```bash
python3 -m http.server   # or: npx serve
```

**Cloudflare deployment** (via wrangler — `wrangler.jsonc` serves the repo root as static assets):
```bash
npx wrangler deploy
npx wrangler dev         # local preview through Workers runtime
```

There are no tests, linters, or formatters configured.

**Whitespace check**:
```bash
git diff --check
```

## Architecture

- `/index.html` — landing page / artist directory. Uses the global CSS (`assets/css/retro-theme.css` + `assets/css/styles.css`) and the shared retro/HUD theme.
  - The active homepage layout is `.page-container > .hud-shell`.
  - Desktop placement is mostly absolute inside `.hud-shell`: `.title-console`, `.content` / `.statement-panel`, four `.orbit-panel` artist links, the outer frame overlay, and `footer.site-footer`.
  - Mobile breakpoints at `980px` and `560px` switch the HUD pieces back into a simpler grid and use bordered neon panels instead of large raster frames.
- `/artists/<name>/` — one room per artist (Jim, Livi, Loy, Symoné). Two files:
  - `index.html` — markup + the section-switching `<script>` only. Links exactly one stylesheet: `<link rel="stylesheet" href="assets/theme.css"/>`.
  - `assets/theme.css` — the artist's complete self-contained theme. Each defines its own `:root` with three scales: **color** (semantic names per artist — `--acid`/`--pink` for Loy, `--cloud-border`/`--teal-bright` for Livi, etc.), **font-size** (`--fs-1` … `--fs-N`), **spacing** (`--sp-1` … `--sp-N`). Font-faces and asset references go in `theme.css` and reach shared assets via `../../../assets/...`.
- Artist themes intentionally do not link the homepage stylesheets — the four rooms each have a distinct aesthetic that would clash with the global retro theme.
- `/assets/` — shared fonts (W95FA, Pixeltype, the CursedGothic family under `cursed_gothic/`), images, JS, and the two-file global CSS system used **only by the homepage**:
  - `styles.css` — HUD layout, frame placement, responsive behavior, background layers, and spacing.
  - `retro-theme.css` — colors, typography, scanline/flicker animations, and chrome/glow variables. Defines the `:root` CSS variables (`--bg-color`, `--text-color`, `--accent-color`, `--highlight-color`, `--border-color`, `--panel-bg`, `--panel-bg-soft`, `--glow-green`, `--glow-blue`, `--shadow-color`, etc.).
- `/assets/frames/` — source frame images and processed chrome overlays:
  - Source UUID files are preserved.
  - Transparent web-ready overlays live in `/assets/frames/processed/`.
  - The homepage currently uses `frame-outer.png`, `frame-title.png`, `frame-statement-alt.png`, `frame-orb.png`, and the opaque `3aa6ac7c-6644-4e39-a35e-edb2f3d0e2e0.png` inner backdrop.
- `.assetsignore` — keeps Cloudflare from uploading `.git`, local tooling files, and other non-site assets when the repo root is served as static assets.

## Hard Rules (from AGENTS.md)

- **Never overwrite or replace existing artist copy / descriptions** with placeholders. Preserve the exact text in the HTML.
- **Homepage**: keep the centered chrome/HUD composition. Do not convert it to a generic landing page, card grid, or plain centered text layout.
- **Homepage background**: the current star field intentionally repeats, stays fixed, and flickers through `body::before`; do not restore the older no-repeat behavior unless asked.
- **Homepage colors**: don't hardcode colors in homepage CSS when a variable exists in `retro-theme.css`.
- **Artist rooms**: don't reintroduce inline `<style>` blocks or link the homepage stylesheets — each room's styling lives entirely in its `theme.css`. Don't mix raw literals with the var-driven scales: if a value has a `--fs-*` / `--sp-*` / color var, use the var. One-off decorative literals (gradient stops, rgba glow tints) stay inline by design.
- Section-switching uses a small inline `<script>` that toggles `.active` on nav items and panels; keep that pattern when adding sections.
- See `STYLE.md` before adjusting homepage positioning values.
