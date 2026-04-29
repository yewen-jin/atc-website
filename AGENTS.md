# Agent Instructions for `atc-website`

This is a vanilla HTML/CSS/JS website for the "Accept the Cookies" arts collective. It follows a 90s retro/cybernetic aesthetic and does not use any build tools, package managers, or JS frameworks (no React, no Svelte).

## Critical Rules
- **NEVER overwrite, replace, or modify user-provided content or artist descriptions** with placeholders. Always preserve the exact copy that currently exists in the HTML files.
- **Layout Constraints:** The design is intentionally left-aligned with a constrained max-width (e.g., 600px container) against a fixed background image. Do not center everything or change it to full-width card grids.
- **Background Images:** Global background images should keep their original size (`background-size: auto;`), not repeat (`background-repeat: no-repeat;`), and stay fixed (`background-attachment: fixed;`), with a fallback background color filling the empty space.

## Architecture & Structure
- **No Build Step:** The site is served entirely as static HTML/CSS files. To preview, open `index.html` in a browser or run a simple local server (e.g., `python3 -m http.server` or `npx serve`).
- **Main Entrypoint:** `/index.html` serves as the directory for the different artists.
- **Artist Rooms:** Each artist has their own dedicated folder under `artists/` (e.g., `artists/jim/index.html`). **Each artist room defines its own complete visual world inline** — its own `<style>` block, fonts, colors, and layout. Artist rooms intentionally do **not** link the homepage stylesheets; the four rooms (Jim, Livi, Loy, Symoné) each have a distinct aesthetic that would clash with the global retro theme. Artist rooms may reference shared font/image assets under `/assets/` directly via `../../assets/...`.
- **Shared Assets:** All global fonts, images, and CSS are in `/assets/`.

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

### Artist rooms — self-contained per-artist aesthetic
- Each room has its own inline `<style>` block — colors, fonts, decorative elements are scoped to that page and chosen to express that artist's character.
- Don't try to "harmonise" rooms with the global theme variables; they are deliberately distinct.
- Each room loads only the fonts it actually uses via `@font-face` from `../../assets/fonts/...`.
- Section-switching is done with a small inline `<script>` that toggles `.active` on nav items and `.section` panels — keep this pattern when adding new sections.

## Workflows & Adding Content
- When adding a new artist, create a directory in `artists/`, give them an `index.html` with its own self-contained `<style>` and visual identity.
- The project is deployed via standard static hosting (e.g., Vercel or Cloudflare Pages) where the repository root is the publish directory.