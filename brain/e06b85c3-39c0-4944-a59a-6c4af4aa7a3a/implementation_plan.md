# Math Visualiser — Logic & Visual Fixes

## Summary

A comprehensive audit of the codebase revealed both **logical bugs** and **visual/layout defects** across the rendering pipeline. The fixes preserve the dark editorial vibe while making the math canvas Manim-style (clean, precise, always readable).

---

## Problems Found

### 🔴 Critical Logic Bugs

1. **`AnimatedPolygon` used as a line** (qrLesson step 3)
   - A 2-vertex polygon (`[[1,2],[1.6,0.8]]`) is treated as a fill shape but renders as nothing useful. It should be a dashed line.

2. **`AnimatedMath` overlap / off-screen positioning**
   - `coords` are in *world units* but after camera transform they can land on top of the Overlay panel or each other. No collision avoidance.
   - The clamp is only `2–98%` which is too loose — blocks overlap the sidebar and timeline.

3. **`useSceneTween` deep-lerp on string IDs / colors**
   - When a new step introduces elements that didn't exist in the previous step, `deepLerp` receives `undefined` → `[number, number]` causing NaN coordinates mid-transition.
   - Specifically: interpolating from `undefined` to an object doesn't correctly early-create the element — it snaps at t≥0.5 but the geometry fields may be NaN before then.

4. **Camera shift causes MathBlock repositioning jitter**
   - `AnimatedMath` derives screen% from the current `camera` prop, but `useSceneTween` interpolates the camera too. During transition, the block slides erroneously as camera animates.

5. **`AnimatedGrid` axis stroke colors too faint**
   - Axis lines use `#444` but after the camera scale of 40–60px/unit, the "axis" lines visually disappear on the dark `#0f0f11` background. Manim uses clearly visible whitish axes.

6. **`SVGCanvas` uses `preserveAspectRatio="none"`**
   - This distorts circles (origin dot becomes ellipse) and arrowheads at non-1:1 aspect ratios. Should use `xMidYMid meet` or handle via camera group composition.

### 🟡 Visual / Layout Defects

7. **Overlay panel always animates in** (via CSS `animation: overlay-in`)
   - On step changes the panel re-triggers its entrance animation if a key changes. Should smoothly cross-fade text instead.

8. **Math equations in Overlay have no fade-in**
   - The KaTeX equations snap in immediately when step changes; they should fade.

9. **`AnimatedVector` label positioning**
   - The label `transform` uses the raw `(x + offset)` but for vectors near the canvas edge, labels get clipped. No edge-awareness.

10. **`AnimatedAngle` min-points check is wrong**
    - Guards `if (points.length < 2)` but a right-angle marker needs exactly 3 points. Should guard for `< 3`.

11. **`AnimatedPolygon` edge rendering: no stroke on fill-only polygons**
    - When `strokeColor` is absent, `strokeWidth` is 0, making polygon outlines invisible. Needs a subtle outline for visibility.

12. **Grid is too dense / too faint** — The GRID_EXTENT=12 with zoom=55–60 means ~25 grid lines are crammed in a small space. Should be bounded by viewport size.

13. **`JSONPanel` textarea** has no syntax highlighting and is completely monochrome — functional but jarring. Minor aesthetic issue.

14. **Timeline scrubber dots can overlap the fill bar** — The ring-[3px] rings on the dots expand outside their space at small step counts.

---

## Proposed Changes

### Component Layer

#### [MODIFY] AnimatedGrid.tsx
- Draw axes (x=0, y=0) as bright white (`rgba(255,255,255,0.5)`) lines
- Draw minor grid in `rgba(255,255,255,0.05)` — subtler
- Cap grid extent dynamically based on viewport/zoom to avoid density overload

#### [MODIFY] AnimatedAngle.tsx  
- Fix guard: `if (points.length < 3)` instead of `< 2`
- Ensure the right-angle square is drawn correctly as a polyline

#### [MODIFY] AnimatedVector.tsx
- Keep arrowhead correct even at small vector lengths (clamp minimum shaft length)
- Improve label positioning: clamp label to stay within `±4` world units from origin

#### [MODIFY] AnimatedPolygon.tsx
- Add a thin semi-transparent stroke (`rgba(color, 0.6)`) even on fill-only polygons for edge definition
- Handle 2-vertex degenerate case gracefully (render as a `<line>` instead)

#### [MODIFY] AnimatedMath.tsx
- Clamp to `8–72%` X range (avoids sidebar collision) and `8–80%` Y range (avoids timeline)
- Add `transition: opacity 0.4s, left 0.3s, top 0.3s` for smooth repositioning during camera moves
- Stop animating position during camera transitions (hold position until camera settles)

#### [MODIFY] AnimatedLine.tsx
- Support `strokeWidth` prop so dashed projection lines can be 1px instead of always 2px

#### [MODIFY] SVGCanvas.tsx
- Fix `preserveAspectRatio` — use `xMidYMid meet` to prevent distortion
- Alternatively keep `none` but add `vectorEffect="non-scaling-stroke"` universally (already done on most shapes — this is correct, we just need the aspect ratio fix)
- Actually the correct fix: keep `preserveAspectRatio="none"` (it's intentional for 1:1 pixel mapping) but add `rx` scaling compensator for the origin circle

#### [MODIFY] Overlay.tsx
- Remove re-triggering CSS animation (use `key` only on the inner content, not the wrapper)
- Add smooth cross-fade for title/description/equations when step changes

#### [MODIFY] MathText.tsx
- Add fade-in animation when KaTeX equation content changes

### Hook Layer

#### [MODIFY] useTween.ts (`useSceneTween`)
- **Key fix**: when a vector/polygon/line appears in `to` but not in `from`, initialise it from `to` with `opacity: 0` so it fades in cleanly rather than NaN-teleporting
- When an element disappears (in `from` but not `to`), fade it out by lerping opacity to 0

### Data Layer

#### [MODIFY] orthoLesson.ts
- Fix `mathBlocks` coords — `[6, 2]` and `[7, 2]` map to ~right edge of screen at zoom=60 — these fall off-screen or under the right vignette. Change to world coords that stay visible.
- `camera.centerX: -150, 100, 150` in pixel-space collide with the overlay panel. Audit camera positions.

#### [MODIFY] qrLesson.ts
- Fix the projection line in step 3 — currently uses `AnimatedPolygon` with 2 vertices. Replace with `lines` entry.

### CSS Layer

#### [MODIFY] index.css
- Add `.overlay-content` fade-in keyframe for text transitions
- Refine grid coloring variables
- Add `math-canvas-block` transition utility

---

## Verification Plan

1. Run dev server and step through all steps in both lessons
2. Verify: no elements overlap the Overlay panel
3. Verify: math blocks stay on-screen at all camera positions  
4. Verify: smooth fade-in of new elements and fade-out of removed ones
5. Verify: right-angle indicator renders as a square bracket (3 points)
6. Verify: grid axes are clearly visible, minor lines don't overwhelm
7. Verify: no NaN/black-flash during step transitions
