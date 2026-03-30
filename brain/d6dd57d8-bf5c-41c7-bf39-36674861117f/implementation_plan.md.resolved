# Implementation Plan - Editorial Overhaul & Layout Refinement

We will transform the "Universal Math Engine" into a sleek, **Dark Mode Editorial** experience. This involves moving the UI into "safe zones" to prevent blocking the graph, implementing reactive sizing for the canvas, and adopting a premium black-and-white aesthetic with bold math accents.

## User Review Required

> [!IMPORTANT]
> To prevent blocking, we will move the "Step Description" into a structured sidebar or a non-overlapping floating panel. This changes the layout from "Overlay-on-Canvas" to "Structured Canvas Layout".

> [!CAUTION]
> The "Editorial" look uses pure black (`#000`) and high-contrast whites. We will ensure that the existing math colors (Yellow, Blue, Green, Red) remain legible against this stark background.

## Proposed Changes

### [Aesthetics]
#### [MODIFY] [index.css](file:///Users/oli/Desktop/MathLessonVisualiser/src/index.css)
- Update color tokens to "Editorial" palette:
  - `bg`: `#000000` (Pure Black)
  - `surface`: `#0a0a0c` (Very Dark Gray)
  - `text`: `#ffffff` (Pure White)
  - `text-muted`: `#888888`
  - `accent`: `#fff`
- Add high-end typography settings (Inter/Outfit).
- Refine animations with `cubic-bezier(.16,1,.3,1)` (Quintic Out) for that "premium" feel.

### [Layout & Reactivity]
#### [MODIFY] [App.tsx](file:///Users/oli/Desktop/MathLessonVisualiser/src/App.tsx)
- **Safe Area Implementation**: 
  - Add logic to calculate a "Safe Center" for the SVG based on the header and sidebar dimensions.
  - Instead of `absolute top-8 left-8` for the description, use a layout that pushes the SVG to the remaining space.
- **Reactive Zooming**:
  - Add a `useWindowResize` hook (or simple state) to adjust the `zoom` level based on the viewport width (e.g. `zoom = Math.min(width/15, 60)`).
- **Icon Refresh**:
  - Update Lucide icons to use thinner strokes (`stroke-width="1.5"`) and consistent white colors.
- **Overlap Prevention**:
  - Update `AnimatedMath` to use a more stable coordinate mapping that accounts for the canvas scale.

### [Engine Refinements]
#### [MODIFY] [App.tsx (AnimatedVector/AnimatedMath)]
- Refine `AnimatedVector` arrowheads to be even sleeker (thinner lines).
- Update the "MathCard" HTML template to follow the "Editorial" B&W look (black background, white borders, crisp text).

## Open Questions
- Would you prefer the **Description** to be a fixed sidebar on the left (taking up ~25% width) or a floating panel that can be toggled/collapsed?
- Should the **Timeline/Player Controls** be a floating island at the bottom (Manim-style) or a full-width bar?

## Verification Plan

### Automated Tests
- `npm run build` to ensure no regression in types or build.

### Manual Verification
- Resize the browser window to verify the graph scales reactively.
- Verify that the description box no longer overlaps the center of the graph (where vectors are usually located).
- Confirm the new color palette feels "Sleek" and "Editorial".
