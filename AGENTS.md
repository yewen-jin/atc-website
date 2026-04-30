# Agent Instructions for `atc-website`

This is a vanilla HTML/CSS/JS website for the "Accept the Cookies" arts collective. It follows a 90s retro/cybernetic aesthetic and does not use any build tools, package managers, or JS frameworks (no React, no Svelte).

## Critical Rules
- **NEVER overwrite, replace, or modify user-provided content or artist descriptions** with placeholders. Always preserve the exact copy that currently exists in the HTML files.
- **Layout Constraints:** The design is intentionally left-aligned with a constrained max-width (e.g., 600px container) against a fixed background image. Do not center everything or change it to full-width card grids.
- **Background Images:** Global background images should keep their original size (`background-size: auto;`), not repeat (`background-repeat: no-repeat;`), and stay fixed (`background-attachment: fixed;`), with a fallback background color filling the empty space.

## Architecture & Structure
- **No Build Step:** The site is served entirely as static HTML/CSS files. To preview, open `index.html` in a browser or run a simple local server (e.g., `python3 -m http.server` or `npx serve`).
- **Main Entrypoint:** `/index.html` serves as the directory for the different artists.
- **Artist Rooms:** Each artist has their own dedicated folder under `artists/<name>/`, containing:
  - `index.html` — markup only; references its theme via `<link rel="stylesheet" href="assets/theme.css"/>`.
  - `assets/theme.css` — the artist's complete self-contained theme (colors, fonts, layout, animations). The four rooms (Jim, Livi, Loy, Symoné) each have a distinct visual identity that would clash with the homepage's retro theme, so artist themes intentionally do **not** link the homepage stylesheets.
  - Per-artist themes reference shared fonts/images under `/assets/` via `../../../assets/...` (three levels up because `theme.css` lives one level deeper than `index.html`).
- **Shared Assets:** All global fonts, images, and the homepage CSS live under `/assets/`.

## Styling & Conventions

### Homepage (`/index.html`) — global retro theme
- **Two-File CSS System:**
  - `assets/css/styles.css`: Manages core layout, spacing, and left-aligned constraints.
  - `assets/css/retro-theme.css`: Manages the retro theme specifics (colors, typography, animations like scanlines and flicker).
- **CSS Variables:** Never hardcode colors on the homepage. Use the variables defined in the `:root` of `retro-theme.css`:
  - `--bg-color` (Backgrounds)
  - `--text-color` (Primary text)
  - `--accent-color` (Headings, primary accents)
  - `--highlight-color` (Hovers, glows)
  - `--border-color` (Borders, links)
- **Typography:** The homepage uses `'W95Fa', 'Courier New', monospace`.

### Artist rooms — self-contained per-artist theme
- Each room's full styling lives in `artists/<name>/assets/theme.css`. Don't reintroduce inline `<style>` blocks, and don't link the homepage stylesheets from artist pages.
- Don't try to "harmonise" rooms with the global theme variables; they are deliberately distinct.
- Every `theme.css` defines its own `:root` with three scales — color (semantic names like `--acid`, `--cloud-border`, `--video-bg`), font-size (`--fs-1` … `--fs-N`), and spacing (`--sp-1` … `--sp-N`). Reference these vars; don't reintroduce raw hex/rem/px literals for values that already have a var. One-off decorative literals (gradient stops, rgba glow tints) are intentionally left inline — they're bespoke per-element art, not part of the theme scale.
- The font-size and spacing scales are exhaustive (every distinct value preserved), not minimal. If you tighten the scale during fine-tuning, update every var-referencing rule rather than mixing vars with literals.
- Section-switching is done with a small inline `<script>` that toggles `.active` on nav items and `.section` panels — keep this pattern when adding new sections.

## Workflows & Adding Content
- When adding a new artist:
  1. Create `artists/<name>/index.html` containing markup and the section-switching script only.
  2. Create `artists/<name>/assets/theme.css` with `@font-face` declarations, a `:root` block defining color / font-size / spacing scales, and the rest of the room's styles using those vars.
  3. Add a link to `artists/<name>/index.html` in the homepage's artist list.
- The project is deployed via standard static hosting (e.g., Vercel or Cloudflare Pages) where the repository root is the publish directory.