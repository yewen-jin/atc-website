# ZX Spectrum UI Skill

Use this skill when designing or implementing a UI that should resemble authentic ZX Spectrum software.

## Objective

Produce the **smallest possible interface** that clearly expresses the product state while preserving recognisable ZX Spectrum aesthetics.

Do not turn the request into a general retro-computing redesign.

## Source style

Treat these visual cues as primary:

- 256×192 logical display
- 8×8 character-cell rhythm
- black, white, cyan, yellow, green, red, blue, magenta
- hard rectangular regions
- bitmap monospace text
- reversed-colour selection rows
- text-led menus
- short keyboard instructions
- occasional Sinclair rainbow stripe
- zero soft visual effects

## Required design behaviour

When creating a screen:

1. Identify the minimum information and actions.
2. Put them on one screen if they fit.
3. Use text before icons.
4. Use a list before cards.
5. Use a highlighted row before a modern button.
6. Use one panel before multiple containers.
7. Use one accent colour at a time where possible.
8. Keep all geometry square and aligned to an 8 px rhythm.
9. Remove anything that exists only to make the UI feel more “designed”.
10. Stop adding components once the task is complete.

## Allowed primitives

Use these by default:

- `Screen`
- `Panel`
- `Text`
- `Menu`
- `MenuItem`
- `KeyHint`
- `StatusBar`
- `Divider`
- `SpectrumStripe`

Do not invent additional primitives unless the task cannot be completed without them.

## Styling rules

```txt
border-radius: 0
box-shadow: none
filter: none
backdrop-filter: none
gradient: none
transition: none, unless required for focus/cursor behaviour
```

Use:

```txt
8 px spacing increments
1–2 px borders
flat fills
high contrast
bitmap/monospace type
full-row selection
```

## Layout rules

Prefer:

```txt
header or system label
main menu/content
single status/help line
```

Do not add a sidebar, top nav, toolbar, footer, or secondary navigation unless the information architecture actually needs one.

## Copy style

Keep UI copy short and operational.

Prefer:

```txt
LOAD
SAVE
EDIT
OPTIONS
SELECT GAME
PRESS ANY KEY
DRIVE M: AVAILABLE.
```

Avoid conversational product copy such as:

```txt
Welcome back!
Let's get started
Here are some things you can do
```

unless explicitly required by the product.

## Interaction rules

Desktop:

```txt
↑ ↓      move
ENTER    select
SPACE    alternate action
ESC      back
A-Z      shortcut where useful
```

Touch/click should mirror the same state model.

Always show keyboard focus.

## Colour use

Default to black/white.

Use cyan for current selection.

Use yellow, green, or red for exceptional emphasis.

Use the Spectrum rainbow only as a compact accent.

Never use all Spectrum colours decoratively across many components.

## Fidelity checks

Before finalising, verify:

- Does it still work at roughly 256×192?
- Are corners square?
- Are there any unnecessary containers?
- Are there any modern card/button patterns that could be plain text or a selected row?
- Is the typography doing most of the visual work?
- Are colours flat and limited?
- Is the interface understandable without decorative effects?

If any answer points toward modern UI polish, simplify again.

## Reference implementation tokens

```css
:root {
  --zx-black: #000;
  --zx-white: #d7d7d7;
  --zx-bright-white: #fff;
  --zx-cyan: #00d7d7;
  --zx-bright-cyan: #00ffff;
  --zx-yellow: #d7d700;
  --zx-green: #00d700;
  --zx-red: #d70000;
  --zx-blue: #0000d7;
  --zx-magenta: #d700d7;

  --zx-grid: 8px;
  --zx-border: 1px;
}
```

## Anti-patterns

Reject by default:

- rounded cards
- shadows
- gradients
- glass
- scanline overlays
- CRT bloom
- smooth animation
- excessive rainbow use
- icon-heavy controls
- decorative pixel art
- generic 1980s neon styling
- unnecessary dashboards
- component-library density

The correct direction is usually **less UI, harder edges, more text**.
