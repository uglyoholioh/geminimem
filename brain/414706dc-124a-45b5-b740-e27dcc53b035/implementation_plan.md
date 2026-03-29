# Plan - Fix Infinite Zoom & Lag

The user is reporting that the graph is "infinitely expanding" and causing lag, and that manual zooming is too slow. This is a critical performance bug caused by a feedback loop between the canvas resolution and its parent's layout.

## User Review Required

> [!CAUTION]
> The lag is likely caused by the canvas buffer size growing unchecked, which can lead to high GPU memory consumption. I will be enforcing strict CSS constraints on the canvas.

## Proposed Changes

### CSS Stabilization & UI
#### [MODIFY] [style.css](file:///Users/oli/Desktop/lectvideoanalyser/MathViz/src/style.css)
- Add `.viz-canvas` styles: `position: absolute; width: 100%; height: 100%; top: 0; left: 0;`.
- Ensure `.canvas-wrapper` has `position: relative` and `overflow: hidden`.
- Add styles for a `.reset-view-btn` floating action button.

### Canvas Optimization & Zoom Control
#### [MODIFY] [canvas-renderer.ts](file:///Users/oli/Desktop/lectvideoanalyser/MathViz/src/renderers/canvas-renderer.ts)
- **Fix Resize Loop**: Remove the resize check from the `requestAnimationFrame` loop. Instead, use a single `ResizeObserver` per canvas that handles buffer resizing reliably.
- **Improved Zoom Speed**: Adjust the scroll `zoomFactor` from 0.92/1.08 to 0.8/1.25 for faster interaction.
- **Safety Clamps**: Clamp the `state.scale` and `targetScale` to a reasonable range (e.g., 0.1 to 1000) to prevent memory-breaking resolutions or "infinite" zooming.
- **Reset Feature**: Export a `resetView(csId)` function and wire it to a UI button.

### Integration
#### [MODIFY] [canvas-renderer.ts](file:///Users/oli/Desktop/lectvideoanalyser/MathViz/src/renderers/canvas-renderer.ts)
- Add the "Reset" button to the `wrapper` in `createInteractiveCanvas`.

## Verification Plan

### Automated Tests
- I will use the `browser` tool to verify that:
  - Resizing the browser window does not cause the canvas to grow indefinitely or trigger a CPU/GPU spike.
  - Using the scroll wheel zooms in and out significantly faster.
  - Clicking the "Reset View" button restores the coordinate system to its default scale and offset.

### Manual Verification
- Ask the user to confirm that the lag has disappeared and that the "Reset View" button helps manage the zoom level effectively.
