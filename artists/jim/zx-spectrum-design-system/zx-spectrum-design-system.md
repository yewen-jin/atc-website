# ZX Spectrum UI Design System

A minimal design system for interfaces inspired by Sinclair ZX Spectrum software, loaders, utilities, and 1980s home-computer menus.

The goal is **not** generic “retro computing”. It should read specifically as **ZX Spectrum**: hard 8×8-grid geometry, bitmap type, flat colour blocks, black/white fields, cyan selection bars, and occasional Sinclair rainbow accents.

---

## 1. Core rules

1. **Use an 8 px grid.**
2. **No rounded corners.**
3. **No shadows, blur, glass, gradients, or soft depth.**
4. **Use one bitmap/monospace face only.**
5. **Keep screens sparse.**
6. **Prefer text over icons.**
7. **Prefer keyboard-style interaction over modern controls.**
8. **Use bright colour only for state, emphasis, or the Sinclair stripe.**
9. **Do not decorate empty space.**
10. **If a component is not necessary to complete the task, omit it.**

---

## 2. Geometry

Reference logical display:

```txt
256 × 192 px
32 × 24 character cells
1 cell = 8 × 8 px
```

For responsive web use, scale the logical canvas by whole or near-whole multiples where practical.

Base spacing:

```txt
--zx-1: 8px;
--zx-2: 16px;
--zx-3: 24px;
--zx-4: 32px;
```

Allowed border widths:

```txt
1px
2px
```

Avoid arbitrary spacing such as 6, 10, 14, 18, 22 px unless required by the bitmap font.

---

## 3. Palette

Use the Spectrum palette, but do not use every colour at once.

```css
:root {
  --zx-black:   #000000;
  --zx-blue:    #0000d7;
  --zx-red:     #d70000;
  --zx-magenta: #d700d7;
  --zx-green:   #00d700;
  --zx-cyan:    #00d7d7;
  --zx-yellow:  #d7d700;
  --zx-white:   #d7d7d7;

  --zx-bright-blue:    #0000ff;
  --zx-bright-red:     #ff0000;
  --zx-bright-magenta: #ff00ff;
  --zx-bright-green:   #00ff00;
  --zx-bright-cyan:    #00ffff;
  --zx-bright-yellow:  #ffff00;
  --zx-bright-white:   #ffffff;
}
```

Default pairings:

```txt
Primary screen       black / white
Utility screen       white / black
Selection            cyan / black
Warning              red / white
System emphasis      yellow / black
Secondary emphasis   green / black
```

Do not use muted “retro” browns, creams, neon synthwave palettes, or CRT-green unless the specific screen calls for them.

---

## 4. Typography

Use one bitmap-style monospace family.

Recommended CSS fallback:

```css
font-family:
  "ZX Spectrum",
  "Pixel Operator",
  "Perfect DOS VGA 437",
  "Courier New",
  monospace;
```

Rules:

```txt
Uppercase is common but not mandatory.
Use normal weight.
Do not use antialiased-looking display typography as decoration.
Line-height: 1.0–1.25.
Letter spacing: 0.
```

Suggested scale:

```txt
body        16 px
small       12–14 px
display     24–32 px, only when genuinely needed
```

Avoid modern type hierarchy with many sizes and weights.

---

## 5. Screen

The screen is the root primitive.

```css
.zx-screen {
  background: var(--zx-black);
  color: var(--zx-bright-white);
  font-family: "ZX Spectrum", monospace;
  image-rendering: pixelated;
}
```

A screen may use:

- black background
- white background
- one flat Spectrum colour background

Do not place textured backgrounds behind UI.

---

## 6. Panel

Use only when content needs a visible boundary.

```css
.zx-panel {
  border: 1px solid currentColor;
  padding: 8px;
  background: var(--zx-white);
  color: var(--zx-black);
}
```

Rules:

- square corners
- flat fill
- no drop shadow
- no floating-card effect
- no nested panels unless structurally necessary

---

## 7. Menu / list

Primary interactive pattern.

```txt
LOADER
+3 BASIC
CALCULATOR
48 BASIC
```

Selected state:

```txt
████████████
 LOADER
████████████
```

Recommended styling:

```css
.zx-menu-item[aria-selected="true"] {
  background: var(--zx-bright-cyan);
  color: var(--zx-black);
}
```

Rules:

- full-row highlight
- no pill selection
- no checkmark unless the state specifically needs one
- use ↑ ↓ ENTER, SPACE, or letter shortcuts where appropriate

---

## 8. Text action

Do not default to modern button chrome.

Preferred:

```txt
ENTER  SELECT
SPACE  BACK
A-Z    JUMP TO LETTER
```

If a clickable control is required, render it as text or a small rectangular key label.

```css
.zx-key {
  display: inline-block;
  border: 1px solid currentColor;
  padding: 0 4px;
  border-radius: 0;
}
```

---

## 9. Status / instruction bar

Use a single-line strip for state or keyboard help.

Examples:

```txt
128 +2A
DRIVE M: AVAILABLE.
STOP THE TAPE and then press any key
```

Use reversed foreground/background when emphasis is needed.

---

## 10. Divider

Use text or a 1 px rule.

```txt
--------------------------------
```

or

```css
border-top: 1px solid currentColor;
```

No ornamental separators.

---

## 11. Sinclair rainbow

Use sparingly as a brand-era accent.

Order:

```txt
red → yellow → green → cyan → blue
```

It may appear as:

- a short diagonal stripe
- a narrow header accent
- a small footer accent

It should never become the page background.

---

## 12. Motion

Default: none.

If motion is necessary, use abrupt state changes:

```txt
blink
hard cut
single-step frame change
cursor flash
```

Avoid easing, spring animation, fades, parallax, and smooth transforms.

---

## 13. Interaction

Prefer:

```txt
Arrow keys   navigate
Enter        confirm
Space        alternate confirm / back
Esc          back
Letter key   jump/select shortcut
```

Mouse/touch can mirror the same actions but should not change the visual language.

Focus must always be visible.

---

## 14. Accessibility

ZX Spectrum aesthetics must not make the interface unreadable.

- Keep text/background contrast high.
- Never rely on colour alone for important state.
- Preserve visible focus.
- Keep interactive text large enough to read.
- Do not simulate CRT blur, scanlines, flicker, chromatic aberration, or screen curvature if they reduce legibility.
- Respect `prefers-reduced-motion`.

---

## 15. Minimal component set

Use only these primitives unless the product genuinely requires more:

```txt
Screen
Panel
Text
Menu/List
Menu Item
Key Hint
Status Bar
Divider
Sinclair Stripe
```

Before adding anything else, ask:

> Can the same task be completed with text, a bordered panel, or a highlighted row?

If yes, do not add a new component.

---

## 16. Avoid

Do not introduce:

```txt
rounded cards
floating cards
badges
chips
avatars
hero sections
carousels
tab bars
accordions
tooltips
dropdown chevrons
hamburger menus
icon buttons
switches
modern input decoration
glassmorphism
skeuomorphic CRT frames
scanline overlays
fake phosphor bloom
pixel-art mascots
generic synthwave styling
```

unless the actual product requirement makes one unavoidable.

---

## 17. Visual test

A successful screen should still feel plausible if shown at 256 × 192 px.

If it relies on polish, gradients, spacing nuance, shadows, rounded containers, or many component types to work, it is not following this system.
