# Homepage Style Notes

The homepage HUD layout is controlled mainly from `assets/css/styles.css`. Theme colors, glows, and typography variables live in `assets/css/retro-theme.css`.

## Current Structure

`index.html` uses this active hierarchy:

```html
<div class="page-container">
  <div class="hud-shell">
    <div class="orbit-panel ...">...</div>
    <div class="title-console">...</div>
    <main class="content">
      <div class="statement-panel">...</div>
    </main>
    <footer class="site-footer">...</footer>
  </div>
</div>
```

The old sigil strip CSS still exists, but the `.sigil-strip` markup is currently commented out.

## Background

```css
body::before {
  background-image: url("../img/background.jpg");
  background-repeat: repeat;
  background-size: 40%;
  background-attachment: fixed;
  background-position: top left;
  animation: bgFlicker 3s infinite alternate;
}
```

- `background-repeat: repeat` spreads the star texture across the viewport.
- `background-size: 40%` controls how dense/large the repeated texture feels.
- `bgFlicker` controls the flickering effect.
- `body::after` is intentionally disabled.

## Main HUD Shell

```css
.hud-shell {
  width: min(100%, 1500px);
  aspect-ratio: 1672 / 941;
  padding: clamp(86px, 8.4vw, 138px) clamp(34px, 4vw, 66px) clamp(92px, 7.8vw, 142px);
  grid-template-columns: minmax(160px, 0.82fr) minmax(430px, 720px) minmax(160px, 0.82fr);
  grid-template-rows: minmax(30px, 0.55fr) minmax(220px, 1.35fr) minmax(90px, 0.55fr) auto;
  background:
    linear-gradient(var(--panel-bg), var(--panel-bg)),
    url("../frames/frame-outer-background.png") center / 100% 100% no-repeat;
}
```

- `width` sets the maximum desktop HUD size.
- `aspect-ratio` keeps the full composition close to the reference image.
- `padding` controls the inner inset from the outer chrome frame.
- The grid is mostly a sizing scaffold on desktop; the major visible pieces are positioned absolutely.
- The inner backdrop image is the opaque main-frame image.

The outer chrome frame is applied with:

```css
.hud-shell::before {
  background: url("../frames/processed/frame-outer-with-footer.png") center / 100% 100% no-repeat;
}
```

## Title Console

```css
.title-console {
  position: absolute;
  top: 6.1%;
  left: 50%;
  width: min(56%, 820px);
  aspect-ratio: 2172 / 724;
  transform: translateX(-50%);
  background: url("../frames/processed/frame-title.png") center / 100% 100% no-repeat;
}
```

- Increase `top` to move the title lower; decrease it to move the title higher.
- `left: 50%` plus `translateX(-50%)` keeps it centered.
- Increase `width` to make the title frame larger.
- `padding` inside `.title-console` controls the logo inset.
- `.site-logo` controls the logo image size and glow.
- `.site-logo transform: translateY(...)` shifts the logo vertically within the title frame without moving the frame-title.png background. Negative values move it up.
- The logo is wrapped in `<a href="/about">` — clicking the logo navigates to the about page.

## Statement Panel

```css
.content {
  position: absolute;
  top: 28.6%;
  left: 50%;
  width: min(48%, 720px);
  transform: translateX(-50%);
}

.statement-panel {
  aspect-ratio: 1448 / 1086;
  padding: 14%;
  background: url("../frames/processed/frame-statement-alt.png") center / 100% 100% no-repeat;
}
```

- Change `.content top` to move the whole statement frame up/down.
- Change `.content width` to make the whole statement frame wider/narrower.
- `left: 50%` plus `translateX(-50%)` keeps the statement group centered.
- `.statement-panel aspect-ratio` should match the current frame asset.
- `.statement-panel padding` controls where the text sits inside the frame.
- `.glowing-text` controls the paragraph size, line-height, and text alignment.
- The statement panel is wrapped in `<a href="/about" class="statement-link">` — clicking it navigates to the about page. `.statement-link` is `display: block; width: 100%` so it does not affect the flex layout of `.content`.

## About Page (`/about`)

Lives at `about/index.html`. It shares the same two CSS files and the full HUD shell (star field background, inner backdrop, `frame-outer-with-footer.png` chrome overlay, `frame-title.png` title console). Differences from the homepage:

- No `.orbit-panel` artist links.
- A wider, taller content area overridden via an inline `<style>` block in the HTML (`.content top: 28%`, `width: min(58%, 860px)`; `.statement-panel aspect-ratio: auto; padding: 8% 12%`).
- The title-console logo links back to `/` (homepage) instead of `/about`.
- Asset paths use `../assets/...` (one level up from `about/`).

## Artist Orbit Buttons

```css
.orbit-panel {
  position: absolute;
}

.orbit-top {
  top: 17.5%;
}

.orbit-bottom {
  top: 55%;
}

.orbit-left {
  left: 7.5%;
}

.orbit-right {
  right: 7.5%;
}

.artist-orb {
  width: clamp(204px, 17.76vw, 293px);
  aspect-ratio: 1;
  background: url("../frames/processed/frame-orb.png") center / 100% 100% no-repeat;
}
```

- Increase `.orbit-top top` to move Loy/Livi lower; decrease it to move them higher.
- Increase `.orbit-bottom top` to move Jim/Symone lower; decrease it to move them higher.
- Adjust `.orbit-left left` and `.orbit-right right` for horizontal spacing.
- Change `.artist-orb width` to grow/shrink all orbit frames from their center.

## Footer

```css
footer.site-footer {
  bottom: clamp(18px, 2.4vw, 42px);
}
```

- Increase `bottom` to move footer text upward.
- Decrease `bottom` to move footer text downward.

## Mobile Breakpoints

At `max-width: 980px`:

- `.hud-shell` becomes a two-column grid.
- The outer frame overlay is hidden.
- `.title-console`, `.content`, and `.orbit-panel` become relative grid items.
- Large raster chrome frames are replaced by bordered neon panels so the layout does not distort.

At `max-width: 560px`:

- The layout stacks into one column.
- Each orbit gets its own row.
- The title and statement panels receive tighter padding and smaller minimum heights.

When changing desktop positioning, check these mobile overrides so the artist links do not overlap the title or statement panel.
