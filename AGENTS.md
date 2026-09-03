# Agent Instructions for `atc-website`

This is a vanilla HTML/CSS/JS website for the "Accept the Cookies" arts collective. It follows a 90s retro/cybernetic aesthetic and does not use any build tools, package managers, or JS frameworks (no React, no Svelte).

## Critical Rules
- **NEVER overwrite, replace, or modify user-provided content or artist descriptions** with placeholders. Always preserve the exact copy that currently exists in the HTML files.
- **Homepage Layout:** The homepage is a centered chrome/HUD composition built from frame image assets. Do not convert it to a generic landing page, card grid, or simple centered text layout. Keep the orb links, title console, statement panel, outer frame, and footer aligned inside `.hud-shell`.
- **Homepage Background:** The current homepage intentionally uses a repeated, fixed, flickering star background in `body::before`. Do not restore the older no-repeat background behavior unless explicitly asked.
- **Frame Assets:** Source frame images live in `assets/frames/`; web-ready transparent overlays live in `assets/frames/processed/`. Rebuild processed overlays from the cleaned source files when frame assets change. The homepage shell uses `frame-outer-background.png` as the opaque inner backdrop and `processed/frame-outer-with-footer.png` as the combined outer/footer chrome overlay.
- **Image Manipulation:** When editing or compositing image assets, create a new output file unless the user explicitly asks to overwrite the original. Do not destructively modify user-supplied source images.
- **Deploy Safety:** The repository root is the publish directory. Keep `.assetsignore` in place so Cloudflare does not upload `.git`, local tooling files, or other non-site assets.

## Architecture & Structure
- **No Build Step:** The site is served entirely as static HTML/CSS files. To preview, open `index.html` in a browser or run a simple local server (e.g., `python3 -m http.server` or `npx serve`).
- **Main Entrypoint:** `/index.html` serves as the directory for the different artists.
- **Homepage HUD:** The homepage markup is organized around `.page-container > .hud-shell`. The shell contains absolute-positioned desktop pieces: `.title-console`, `.content` / `.statement-panel`, four `.orbit-panel` links, the outer frame overlay, and `footer.site-footer`. Mobile breakpoints switch these pieces back into a simpler CSS grid. The site-logo and the statement panel both link to `/about`.
- **About Page:** `about/index.html` serves at `/about`. It reuses the same two CSS files and the full HUD shell (background, inner backdrop, `frame-outer-with-footer.png`, `frame-title.png`) but has no orbit panels. Page-specific layout overrides (wider/taller content area) live in an inline `<style>` block inside `about/index.html`. The logo on this page links back to `/`. Asset paths use `../assets/...`.
- **Artist Rooms:** Each artist has their own dedicated folder under `artists/<name>/`, containing:
  - `index.html` — markup only; references its theme via `<link rel="stylesheet" href="assets/theme.css"/>`.
  - `assets/theme.css` — the artist's complete self-contained theme (colors, fonts, layout, animations). The four rooms (Jim, Livi, Loy, Symoné) each have a distinct visual identity that would clash with the homepage's retro theme, so artist themes intentionally do **not** link the homepage stylesheets.
  - Per-artist themes reference shared fonts/images under `/assets/` via `../../../assets/...` (three levels up because `theme.css` lives one level deeper than `index.html`).
- **Shared Assets:** All global fonts, images, homepage CSS, and frame assets live under `/assets/`.
- **Livi Room:** `artists/livi/` has its own frame-based layout. The desktop page uses `artists/livi/assets/frames/background.png` as the page background and `artists/livi/assets/frames/frame.png` as a non-interactive chrome overlay on `.livi-console::after`. The three nav buttons (`Gallery`, `Tablet`, `About`) are absolutely positioned inside the left frame holes using `button-1.png`, `button-2.png`, and `button-3.png`. The content fill sits below the chrome via z-index, and `.console-title` sits above the frame at the top. The fixed top-left `.back-link` home button reuses the same pushbutton language with `button-4.png`; on mobile the pill nav is pushed down by `--sp-9` so it clears that button. The Gallery uses CSS thumbnails and `left.png` / `right.png` image controls, with keyboard ArrowLeft/ArrowRight support in the inline script. The Tablet section is a `.tablet-layout` two-column grid — copy plus the `.tablet-pair` "bean guys" on the left, the `.image-pair` install view stacked on the right, both inside the one scrolling `.text-panel`, with the quotes spanning below; under `980px` it collapses to one column, and the install pair takes `order: -1` so it sits above the copy and goes back to a side-by-side row (DOM order stays copy-first, which is what desktop and screen readers follow). Both install shots print at one shared width (`.image-pair .image-figure` is `min(100%, 300px)`, image `width: 100%; height: auto`), so the landscape and portrait photos line up on both edges and only their heights differ — `.installation-view .featured-image` sets `max-height: none` to lift the base `.featured-image` cap. `.text-panel` styles its own chrome-rail scrollbar through `::-webkit-scrollbar`; the Firefox `scrollbar-width`/`scrollbar-color` fallback is deliberately wrapped in `@supports not selector(::-webkit-scrollbar)`, because setting those in Chrome switches the gradient thumb off. Image captions are not rendered in Livi's room — each `img` carries a descriptive `alt` that absorbs what the caption used to say (the lightbox keeps its own `.lightbox-caption`).

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
- Livi's gallery script additionally maintains `currentSlide`; keep thumbnails, `data-slide-step` controls, and keyboard arrows routed through the same `showSlide()` function so the active thumbnail and caption stay synchronized.

- **Loy Room Themes:**
  - `artists/loy/assets/theme.css` is the active stylesheet for Loy's room and subprojects.
  - `artists/loy/assets/theme-light.css` contains the complete white background light theme.
  - `artists/loy/assets/theme-dark.css` contains the complete dark background dark theme.
  - To switch between dark and light themes at any time, overwrite `theme.css` with the respective backup stylesheet (`theme-dark.css` or `theme-light.css`).

## Workflows & Adding Content
- When adding a new artist:
  1. Create `artists/<name>/index.html` containing markup and the section-switching script only.
  2. Create `artists/<name>/assets/theme.css` with `@font-face` declarations, a `:root` block defining color / font-size / spacing scales, and the rest of the room's styles using those vars.
  3. Add a link to `artists/<name>/index.html` in the homepage's artist list.
- The project is deployed via standard static hosting (e.g., Vercel or Cloudflare Pages / Workers static assets) where the repository root is the publish directory.
- After changing CSS or docs, run `git diff --check` to catch whitespace errors.
