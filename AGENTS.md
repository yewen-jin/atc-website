# Agent Instructions for `atc-website`

This is a vanilla HTML/CSS/JS website for the "Accept the Cookies" arts collective. It follows a 90s retro/cybernetic aesthetic and does not use any build tools, package managers, or JS frameworks (no React, no Svelte).

## Critical Rules
- **NEVER overwrite, replace, or modify user-provided content or artist descriptions** with placeholders. Always preserve the exact copy that currently exists in the HTML files.
- **Homepage Layout:** The homepage is a centered chrome/HUD composition built from frame image assets. Do not convert it to a generic landing page, card grid, or simple centered text layout. Keep the orb links, title console, statement panel, outer frame, and footer aligned inside `.hud-shell`.
- **Homepage Background:** The current homepage intentionally uses a repeated, fixed, flickering star background in `body::before`. Do not restore the older no-repeat background behavior unless explicitly asked.
- **Frame Assets:** Source frame images live in `assets/frames/`; web-ready transparent overlays live in `assets/frames/processed/`. Preserve source UUID files and reference processed files for chrome overlays unless the asset is intentionally opaque, such as the main inner frame backdrop.
- **Deploy Safety:** The repository root is the publish directory. Keep `.assetsignore` in place so Cloudflare does not upload `.git`, local tooling files, or other non-site assets.

## Architecture & Structure
- **No Build Step:** The site is served entirely as static HTML/CSS files. To preview, open `index.html` in a browser or run a simple local server (e.g., `python3 -m http.server` or `npx serve`).
- **Main Entrypoint:** `/index.html` serves as the directory for the different artists.
- **Homepage HUD:** The homepage markup is organized around `.page-container > .hud-shell`. The shell contains absolute-positioned desktop pieces: `.title-console`, `.content` / `.statement-panel`, four `.orbit-panel` links, the outer frame overlay, and `footer.site-footer`. Mobile breakpoints switch these pieces back into a simpler CSS grid.
- **Artist Rooms:** Each artist has their own dedicated folder under `artists/<name>/`, containing:
  - `index.html` — markup only; references its theme via `<link rel="stylesheet" href="assets/theme.css"/>`.
  - `assets/theme.css` — the artist's complete self-contained theme (colors, fonts, layout, animations). The four rooms (Jim, Livi, Loy, Symoné) each have a distinct visual identity that would clash with the homepage's retro theme, so artist themes intentionally do **not** link the homepage stylesheets.
  - Per-artist themes reference shared fonts/images under `/assets/` via `../../../assets/...` (three levels up because `theme.css` lives one level deeper than `index.html`).
- **Shared Assets:** All global fonts, images, homepage CSS, and frame assets live under `/assets/`.

## Styling & Conventions

### Homepage (`/index.html`) — global retro theme
- **Two-File CSS System:**
  - `assets/css/styles.css`: Manages the HUD layout, frame placement, responsive behavior, background layers, and spacing.
  - `assets/css/retro-theme.css`: Manages the retro theme specifics (colors, typography, chrome/glow variables, scanlines, and flicker animations).
- **CSS Variables:** Never hardcode colors on the homepage. Use the variables defined in the `:root` of `retro-theme.css`:
  - `--bg-color` (Backgrounds)
  - `--text-color` (Primary text)
  - `--accent-color` (Headings, primary accents)
  - `--highlight-color` (Hovers, glows)
  - `--border-color` (Borders, links)
  - `--panel-bg`, `--panel-bg-soft`, `--glow-green`, `--glow-blue`, `--shadow-color`, and the chrome/glass variables for HUD surfaces.
- **Typography:** The homepage uses `'W95Fa', 'Courier New', monospace`.
- **Homepage Positioning:** Desktop homepage alignment is controlled with percentage `top`, `left`, `right`, `width`, `aspect-ratio`, and `padding` values on `.title-console`, `.content`, `.statement-panel`, and `.orbit-*`. See `STYLE.md` before changing these values.
- **Mobile Homepage:** The mobile breakpoints at `980px` and `560px` intentionally drop the large raster chrome frames and use bordered neon panels to avoid distortion and overlap.

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
- The project is deployed via standard static hosting (e.g., Vercel or Cloudflare Pages / Workers static assets) where the repository root is the publish directory.
- After changing CSS or docs, run `git diff --check` to catch whitespace errors.
