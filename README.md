# Accept the Cookies - Arts Collective Website

A 90s retro/cybernetic styled website for the "Accept the Cookies" arts collective, built with vanilla HTML/CSS/JS - no frameworks, no build tools.

## 🌟 Overview

This is a nostalgic tribute to early web aesthetics featuring glitch art, neon glows, and retro-futuristic design elements. Each artist has their own themed room with unique visual identities while maintaining the collective's cybernetic aesthetic.

## 📁 Project Structure

```
atc-website/
├── index.html              # Main entrypoint & homepage
├── about/                  # About page (retro HUD theme)
├── artists/                # Individual artist rooms
│   ├── jim/                # Jim's themed room
│   ├── livi/               # Livi's themed room
│   ├── loyalty/            # Loy's themed room
│   └── symone/             # Symoné's themed room
├── assets/                 # Shared assets
│   ├── css/                # Global stylesheets
│   │   ├── styles.css      # HUD layout & positioning
│   │   └── retro-theme.css # Retro theme & colors
│   ├── frames/             # Source frame images
│   ├── frames/processed/   # Web-ready transparent overlays
│   ├── fonts/              # W95Fa & other fonts
│   ├── img/                # Backgrounds & images
│   ├── logo/               # Brand assets
│   └── js/                 # JavaScript files
├── .assetsignore          # Cloudflare deployment filter
└── AGENTS.md              # Development instructions
```

## 🎨 Design System

### Homepage (Retro HUD Theme)
- **Two-File CSS System:**
  - `assets/css/styles.css` - HUD layout, frame placement, responsive behavior
  - `assets/css/retro-theme.css` - Colors, typography, chrome effects, animations
- **Color Variables:** Use CSS variables from `retro-theme.css:root`
  - `--bg-color`, `--text-color`, `--accent-color`, `--highlight-color`, `--border-color`
  - `--panel-bg`, `--panel-bg-soft`, `--glow-green`, `--glow-blue`, `--shadow-color`
- **Typography:** `'W95Fa', 'Courier New', monospace`
- **Animations:** Flickering backgrounds, glow effects, scanlines

### Artist Rooms
- Each room has a **self-contained theme** in `artists/<name>/assets/theme.css`
- **No homepage stylesheets** linked from artist pages
- **Unique color scales** with semantic names (`--acid`, `--cloud-border`, `--video-bg`)
- **Complete CSS scales** for colors, font-sizes (`--fs-1` to `--fs-N`), spacing (`--sp-1` to `--sp-N`)
- **Section-switching** via inline JavaScript that toggles `.active` classes

## 🚀 Getting Started

### Local Development
No build step required! Simply:

1. **Open in browser:** `index.html`
2. **Or run local server:**
   ```bash
   python3 -m http.server
   # or
   npx serve
   ```

### URL Structure
- `/` - Homepage with artist navigation
- `/about` - About page with full HUD theme
- `/artists/jim/` - Jim's room
- `/artists/livi/` - Livi's room
- `/artists/loyalty/` - Loy's room
- `/artists/symone/` - Symoné's room

## 📱 Responsive Design

- **Desktop (980px+):** Full HUD chrome frames with orbital navigation
- **Tablet (560px-980px):** Grid layout with neon-bordered panels
- **Mobile (<560px):** Single column stack with touch-friendly orbs

## 🎯 Adding New Artists

1. Create artist directory: `mkdir artists/<name>/`
2. Create `artists/<name>/index.html` with markup + section-switching script
3. Create `artists/<name>/assets/theme.css` with:
   - `@font-face` declarations
   - `:root` block with color/font-size/spacing scales
   - Complete styling using those variables
4. Add link to `artists/<name>/index.html` in homepage artist list

## 🛠️ Development Guidelines

### Critical Rules
- **Never modify** user-provided content or artist descriptions
- **Preserve** the centered chrome/HUD homepage layout
- **Use** `assets/frames/processed/` for web-ready overlays
- **Keep** `.assetsignore` for Cloudflare deployment safety

### CSS Best Practices
- Homepage: **Always use CSS variables** - never hardcode colors
- Artist rooms: **Reference theme variables** - avoid mixing with literals
- **Update all references** when tightening CSS scales
- **Run `git diff --check`** after changes to catch whitespace errors

## 🚀 Deployment

The repository root is the **publish directory**. Deploy to any static hosting:

- **Vercel**
- **Cloudflare Pages/Workers**
- **Netlify**
- **GitHub Pages**

Ensure `.assetsignore` is in place to prevent uploading `.git`, local tooling files, and other non-site assets.

## 🎨 Assets

- **Frames:** Source UUID files in `assets/frames/`, processed overlays in `assets/frames/processed/`
- **Fonts:** W95Fa font in `assets/fonts/`
- **Backgrounds:** Glitch patterns in `assets/img/`
- **Logo:** Brand assets in `assets/logo/`

## 🤝 Contributing

1. Follow the existing aesthetic conventions
2. Maintain the 90s retro/cybernetic vibe
3. Test across all breakpoints
4. Preserve existing content and structure
5. Use the established CSS variable systems

---

Built with vanilla HTML/CSS/JS - no frameworks, no build tools, just pure web nostalgia. 🌟