# Loy Page Redesign — Implementation Plan

Redesign `artists/loy/` from a single JS section-switching page to a two-level static site:
- **Main page** (`artists/loy/index.html`): project index — title, one image, one sentence per project.
- **Per-project pages** (`artists/loy/projects/<slug>/index.html`): full text, all images, credits.
- Credits remains on the main page as a bottom section (no separate page needed).

Reference format: https://onlyslime.net/media

---

## URL & Slug Map

| # | Current section ID | Slug | Project page path |
|---|-------------------|------|-------------------|
| 01 | `section-gatekeeper` | `gatekeeper` | `artists/loy/projects/gatekeeper/index.html` |
| 02 | `section-underground` | `underground` | `artists/loy/projects/underground/index.html` |
| 03 | `section-cringe` | `cringe` | `artists/loy/projects/cringe/index.html` |
| 04 | `section-chapter0` | `chapter0` | `artists/loy/projects/chapter0/index.html` |
| 05 | `section-workshops` | `workshops` | `artists/loy/projects/workshops/index.html` |
| 06 | `section-biennale` | `biennale` | `artists/loy/projects/biennale/index.html` |
| 07 | `section-experiments` | `experiments` | `artists/loy/projects/experiments/index.html` |
| 08 | `section-chapter88` | `chapter88` | `artists/loy/projects/chapter88/index.html` |
| 09 | `section-seraphim` | `seraphim` | `artists/loy/projects/seraphim/index.html` |

---

## Path Rules (important)

- Project pages live two levels deeper than `artists/loy/`. Their stylesheet link must be:
  `<link rel="stylesheet" href="../../assets/theme.css"/>`
- Image `src` paths inside project HTML go from `assets/img/...` → `../../assets/img/...`
  (or keep a symlink — but for static hosting, relative paths are safer).
- **`theme.css` font/image URLs do not change** — they are relative to the CSS file, which stays at `artists/loy/assets/theme.css`.
- The back link on project pages returns to the Loy index: `href="../../index.html"`.

---

## Content Work (do before coding)

- [x] **Write one-sentence summary per project.** One sentence only — shown on the main index card.
      Fill in the table below:

| Slug | One-sentence summary |
|------|---------------------|
| gatekeeper | A CCTV video installation that makes invisible data tracking visible, asking what it really means to click 'accept'. |
| underground | An esoteric exploration of low-tech and no-tech — drawing hand-activated portals in autonomous spaces and asking whether spirit can inhabit a machine. |
| cringe | A deck of 52 questions used to spark over 200 real conversations across the UK about what people want from a better internet. |
| chapter0 | A video installation following an impressionable hacker spiralling into AI-induced psychosis, shown at the Immersive Experience Network Summit. |
| workshops | [Placeholder] Community workshops condensing the full project research process into short, participatory sessions at Edmonton Green Library and Fore Street For All. |
| biennale | [Placeholder] An online exhibition presence through Styly at The Wrong Biennale. |
| experiments | A collection of technical experiments — VR passthrough, TouchDesigner, PlayCanvas — documenting the tools and interfaces behind the work. |
| chapter88 | A moving image piece following a glassy-eyed androgyne drifting into visions of an alternative world behind an empty fish tank, first shown at arebyte Digital Art Centre. |
| seraphim | A 3D and video collaboration imagining biblically accurate angels processing internet code in the backrooms, built in Blender and rendered in TouchDesigner. |

- [x] **Confirm hero image per project.** Each project page already has a `featured-img` — use that same image as the index card thumbnail. Note any exceptions here.

---

## Step-by-Step Implementation

### Step 1 — Build one project page as template
- [x] Create `artists/loy/projects/gatekeeper/index.html`
- [ ] Verify `../../assets/theme.css` loads (open in browser, check fonts + colors)
- [x] Port all text from current `section-gatekeeper` **verbatim** (no paraphrasing — AGENTS.md hard rule)
- [x] Port all images; update `src` paths to `../../assets/1. Gatekeeper/...`
- [x] Port credits `info-panel` for this project
- [x] Add `← back to Loy` link pointing to `../../index.html`
- [x] Confirm lightbox JS works (selector targets `.project-media img, .image-grid img` instead of `.section img`)
- [ ] Cross-check: read old HTML section and new page side-by-side, confirm no text is missing or altered

### Step 2 — Migrate remaining 8 projects
- [x] `underground` — created, text verbatim, image paths updated
- [x] `cringe` — created, text verbatim, image paths updated
- [x] `chapter0` — created, text verbatim, image paths updated
- [x] `workshops` — created, text verbatim (sparse content noted)
- [x] `biennale` — created, text verbatim (sparse content noted)
- [x] `experiments` — created, text verbatim
- [x] `chapter88` — created, text verbatim, image paths updated
- [x] `seraphim` — created, text verbatim, image paths updated

### Step 3 — Redesign main index page
- [x] Replace `.card-strip.card-nav` + JS section-switcher with a project list layout
- [x] Each project entry shows: number, title (as `<a href="projects/<slug>/">`), hero image thumbnail, one-sentence summary
- [x] Add Credits section at the bottom of the main page (no separate page)
- [x] Remove the JS section-switching `<script>` block entirely
- [x] Keep CCTV bar, title block, status bar, and back-to-home link

### Step 4 — Add theme CSS for new index layout
- [x] Add `.project-index` list/grid styles to `assets/theme.css`
- [x] Add `.project-card-*` (title + thumb + summary) styles
- [x] Add `.project-page` spacing rules for standalone project pages
- [x] Fix `var(--mid)` bug in `.credits-line` (undefined var → inline rgba)
- [x] Add `.project-card-body--full` for no-image entries (no inline styles)

### Step 5 — Mobile pass
- [ ] Check index page at 980px and 560px breakpoints
- [ ] Check one project page (gatekeeper) at both breakpoints — verify image sizes, text legibility, back link visibility

### Step 6 — Update AGENTS.md
- [ ] Update the architecture section to document the new two-level structure for Loy's room
- [ ] Note the path depth convention for project pages (`../../assets/theme.css`)
- [ ] Note that the Loy index no longer uses JS section-switching

---

## Progress Summary (last updated: 2026-05-01)

| Step | Status |
|------|--------|
| Content work (summaries + hero images) | ✅ Done |
| Step 1 — Gatekeeper template page | ✅ Done (browser verify pending) |
| Step 2 — Migrate 8 remaining projects | ✅ Done |
| Step 3 — Redesign main index | ✅ Done |
| Step 4 — Theme CSS additions + bug fix | ✅ Done |
| Step 5 — Mobile pass | ⏳ Pending |
| Step 6 — Update AGENTS.md | ⏳ Pending |

---

## Definition of Done

- All 9 project pages exist and render with correct styles and images
- All text matches the original content verbatim
- Main index page links to all 9 project pages and shows credits at bottom
- No broken image or stylesheet paths
- Mobile layout works on both the index and project pages
- AGENTS.md updated

---

## Notes & Decisions Log

_Record any decisions made during implementation here so future sessions have context._

- Credits (section 10) stays on main page, not a separate project page.
- Lightbox JS on project pages: change selector from `.section img` to `.project-media img, .image-grid img`.
