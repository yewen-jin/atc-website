# Agent Instructions for `atc-website`

This is a vanilla HTML/CSS/JS website for the "Accept the Cookies" arts collective. It follows a 90s retro/cybernetic aesthetic and does not use any build tools, package managers, or JS frameworks (no React, no Svelte).

## Architecture & Structure
- **No Build Step:** The site is served entirely as static HTML/CSS files. To preview, simply open `index.html` in a browser or run a simple local server (e.g., `python3 -m http.server` or `npx serve`).
- **Main Entrypoint:** `/index.html` serves as the directory for the different artists.
- **Artist Rooms:** Each artist has their own dedicated folder under `artists/` (e.g., `artists/jim/index.html`). Artist pages are allowed to have unique HTML layouts and custom CSS files (placed in `artists/<name>/assets/`), but they must still incorporate the global CSS to maintain the core aesthetic.
- **Shared Assets:** All global fonts, images, and CSS are in `/assets/`.

## Styling & Conventions
- **Two-File CSS System:** 
  - `assets/css/styles.css`: Manages layout, responsive grids, and spacing.
  - `assets/css/retro-theme.css`: Manages the retro theme specifics (colors, typography, animations like scanlines and flicker).
- **CSS Variables:** Never hardcode colors. Always use the CSS variables defined in the `:root` of `retro-theme.css`:
  - `--bg-color` (Backgrounds)
  - `--text-color` (Primary text)
  - `--accent-color` (Headings, primary accents)
  - `--highlight-color` (Hovers, glows)
  - `--border-color` (Borders, links)
- **Typography:** The site uses a custom local font (`W95Fa`). Use `'W95Fa', 'Courier New', monospace` for themed text.

## Workflows & Adding Content
- When adding a new artist, create a directory in `artists/`, give them an `index.html` that links back to `../../assets/css/styles.css` and `../../assets/css/retro-theme.css`. 
- Avoid introducing inline styles for anything related to the retro aesthetic; always map it back to CSS variables.
- The project is deployed via standard static hosting (e.g., Vercel or Cloudflare Pages) where the repository root is the publish directory.