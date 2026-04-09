# Dynamic Auto-Framing & Legibility Enhancements

The overall goal is twofold:
1. Make all text, labels, and mathematical visualisations bolder, clearer, and far more visible across both the UI and canvas.
2. Introduce a robust, non-hardcoded "Auto-framing" mechanism (safe-area projection) to ensure that mathematical objects automatically scale, center themselves, and never clip or hide underneath UI overlays.

## Proposed Changes

### 1. Thicker & Clearer Typography
**Component:** `src/app/globals.css`
- Apply global `font-medium` (500 weight) or `font-semibold` overrides to the base HTML/Body style.
- Increase the base `font-size` slightly, and boost letter-spacing to enhance legibility.
- Increase `.katex` font sizes by ~15% inside the visualizer so formulas stand out more.

**Component:** `src/lib/math-engine/renderer.ts`
- **Grid Labels:** Upgrade from standard `10px` to `600 12px` or `bold 12px`.
- **Canvas Objects (Points, Vectors):** Upgrade font references (e.g., from `13px` to `bold 15px`). 
- **Stroke Widths:** Increase default stroke widths for graphs and vectors (e.g., `width=2` to `width=3`) to make lines thicker.

### 2. Dynamic Safeguards (Auto-Framing Coordinate Space)
**Component:** `src/components/math-visualizer/MathCanvas.tsx`
- **Dynamic Bounding Volume Calculation:** 
  We will introduce a helper function `computeSceneBounds(elements)` that iterates over all currently visible elements (Points, Vectors, Functions, Polygons, Circles) and computes an exact mathematical bounding box `[xMin, xMax, yMin, yMax]`.
- **Viewport Safe-Area Padding:**
  When projecting these mathematical boundaries to the `MathCanvas` viewport dimensions, we will apply a dynamic UI "safe area" margin (e.g., reserving space for the top description card, the bottom controls) ensuring elements are never blocked by floating UI pieces.
- **Replacing Hardcoded Defaults:**
  Instead of hardcoding `DEFAULT_X = [-8, 8]`, the camera will automatically span wide enough to contain the exact `SceneBounds`, plus a standard ~15-20% margin, overriding any hardcoded limits naturally.

## Open Questions
- Are you satisfied with simply bumping the weights and sizes of the current *Geist* and system-ui fonts, or would you like to completely swap the application to a thicker font family (like `Space Grotesk`, `Outfit`, or `Inter`)?
- Should the auto-framing transition be instant on each step, or should the camera continuously automatically pan/zoom smoothly between step changes as the bounding box expands?

## Verification Plan
1. **Visual Load Test:** I will open a heavily populated math lesson layout, ensuring all axes, vectors, and text labels render bolder and are clearly legible.
2. **Auto-Framing Test:** I will verify that scattered points or huge bounds are automatically brought clearly into view without user zooming, and verify none of them fall underneath absolute-positioned overlays.
