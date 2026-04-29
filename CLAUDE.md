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

## Architecture

- `/index.html` — landing page / artist directory. Uses the global CSS (`assets/css/retro-theme.css` + `assets/css/styles.css`) and the shared retro theme.
- `/artists/<name>/index.html` — one room per artist. Each room defines its **own complete visual world inline** (its own `<style>` block, fonts, colors, layout). Artist rooms intentionally do not link the global stylesheets — the four artists (Jim, Livi, Loy, Symoné) each have a distinct aesthetic that would clash with the homepage's retro theme. They reference shared font/image assets directly via `../../assets/...`.
- `/assets/` — shared fonts (W95FA, Pixeltype, the CursedGothic family under `cursed_gothic/`), images, JS, and the two-file global CSS system used **only by the homepage**:
  - `styles.css` — layout, spacing, the deliberately left-aligned constrained-width container.
  - `retro-theme.css` — colors, typography, scanline/flicker animations. Defines the `:root` CSS variables (`--bg-color`, `--text-color`, `--accent-color`, `--highlight-color`, `--border-color`).

## Hard Rules (from AGENTS.md)

- **Never overwrite or replace existing artist copy / descriptions** with placeholders. Preserve the exact text in the HTML.
- **Homepage**: don't centralize the layout or convert it to a full-width grid; don't hardcode colors (use the CSS variables from `retro-theme.css`); keep the background image `auto`/`no-repeat`/`fixed` with a fallback color.
- **Artist rooms**: don't try to harmonise them with the homepage's retro theme — each room's distinct visual identity is intentional. Section-switching uses a small inline `<script>` that toggles `.active` on nav items and panels; keep that pattern when adding sections.
