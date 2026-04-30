# Homepage Style Notes

The homepage HUD layout is controlled mainly from `assets/css/styles.css`.

## Main Frame

```css
.hud-shell {
    padding: clamp(86px, 8.4vw, 138px) clamp(34px, 4vw, 66px) clamp(92px, 7.8vw, 142px);
    grid-template-columns: minmax(160px, 0.82fr) minmax(430px, 720px) minmax(160px, 0.82fr);
    grid-template-rows: minmax(130px, 0.55fr) minmax(300px, 1.35fr) minmax(120px, 0.55fr) auto;
}
```

- `padding` controls the inset from the outer chrome frame.
- `grid-template-columns` controls left orbit area, center content width, and right orbit area.
- `grid-template-rows` controls vertical zones for top orbits/title, statement area, lower orbits, and sigil strip.

## Central Content Group

```css
.content {
    grid-column: 2;
    grid-row: 2 / 4;
    justify-content: flex-start;
    gap: clamp(10px, 1.2vw, 20px);
}
```

- `grid-column` places the statement/content stack in the middle column.
- `grid-row` controls how much `hud-shell` grid space the statement/content stack occupies.
- The title frame is not inside `.content`; it is its own direct `hud-shell` grid item.
- `gap` controls spacing inside the content stack.

## Title Frame

```css
.title-console {
    grid-column: 2;
    grid-row: 1;
    justify-self: center;
    width: min(118%, 880px);
    min-height: clamp(175px, 18vw, 260px);
    margin-top: clamp(-6px, -0.3vw, 0px);
    padding: clamp(26px, 3.8vw, 56px) clamp(52px, 5.4vw, 94px);
}
```

- `grid-column: 2` and `grid-row: 1` place it in the center top row of the `hud-shell` grid.
- Increase `width` / `min-height` to make the title frame larger.
- Increase `margin-top` to move it lower.
- Make `margin-top` more negative to move it higher.
- `padding` controls logo inset inside the title frame.

## Statement Frame

```css
.statement-panel {
    min-height: clamp(360px, 31vw, 540px);
    margin: clamp(6px, 0.8vw, 16px) clamp(-64px, -4.2vw, -26px) 0;
    padding: clamp(80px, 8vw, 126px) clamp(96px, 8.8vw, 148px);
}
```

- `min-height` makes the frame taller or shorter.
- First `margin` value moves the frame down/up.
- Negative side margins make the frame wider.
- `padding` controls text inset inside the frame.

## Artist Orbit Buttons

```css
.orbit-panel {
    transform: translateY(clamp(44px, 5vw, 84px));
}
```

- Increase `translateY` to move all orbit buttons lower.
- Decrease `translateY` to move all orbit buttons higher.

```css
.orbit-top {
    grid-row: 2;
}

.orbit-bottom {
    grid-row: 3;
}

.orbit-left {
    grid-column: 1;
}

.orbit-right {
    grid-column: 3;
}
```

- These rules place each orbit into the grid zones.

## Bottom Sigil Strip

```css
.sigil-strip {
    grid-column: 2;
    grid-row: 4;
    width: min(100%, 460px);
}
```

- `grid-column` and `grid-row` place the sigil strip.
- `width` controls its size.

## Footer Text

```css
footer.site-footer {
    bottom: clamp(18px, 2.4vw, 42px);
}
```

- Increase `bottom` to move footer text upward.
- Decrease `bottom` to move footer text downward.

## Background

```css
body::before {
    background-image: url("../img/background.jpg");
    background-repeat: repeat;
    background-size: auto;
    background-attachment: fixed;
    background-position: top left;
    animation: bgFlicker 3s infinite alternate;
}
```

- `background-repeat: repeat` spreads the star texture across the viewport.
- `background-size: auto` keeps the source asset at its natural size.
- `bgFlicker` controls the flickering effect.
