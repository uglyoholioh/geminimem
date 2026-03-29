# Explainer Video Style Transitions

This plan outlines the architecture for moving from "abrupt steps" to a smooth, video-like presentation flow. The core idea is to implement **In-Place Interpolation** where objects persist across steps and transition their properties (position, color, values) over time.

## Proposed Changes

### High-Level Flow Improvement
- **Automatic Transitions**: If an object exists in both the current and next step (matched by `id`), its properties (like vector components) will automatically interpolate over a fixed duration (e.g., 1000ms).
- **Staggered Sequencing**: Animations will be sequenced to feel like a narrative:
  1. **Fade Out** removed elements.
  2. **Animate** transitions of existing elements.
  3. **Fade In** or **Write** new elements.

### 2D/3D Canvas Engine Upgrade

#### [MODIFY] [canvas-renderer.ts](file:///Users/oli/Desktop/lectvideoanalyser/MathViz/src/renderers/canvas-renderer.ts)
- **State interpolation**: Add `prevObjects` and `transitionStartTime` to `CanvasState`.
- **Render Loop Update**: In the 60fps loop, if a transition is active, calculate an interpolation factor `t`. Draw objects using weighted averages of `prevPos` and `targetPos`.
- This will make vectors smoothly "sweep" across the screen and graphs "pan/zoom" gracefully.

### UI Element Reconciliation

#### [MODIFY] [app.ts](file:///Users/oli/Desktop/lectvideoanalyser/MathViz/src/app.ts)
- **Durable Elements**: Refactor `renderBaseState` to avoid `innerHTML = ''`. It will now find existing elements by `id` and update them.
- **Transition Synchronization**: Orchestrate the timing between the text "Write" animation and the graph's visual update.

#### [MODIFY] [matrix-renderer.ts](file:///Users/oli/Desktop/lectvideoanalyser/MathViz/src/renderers/matrix-renderer.ts)
- Add `updateMatrixValues` function to animate numeric changes inside an existing matrix table without re-creating the DOM.

#### [MODIFY] [equation-renderer.ts](file:///Users/oli/Desktop/lectvideoanalyser/MathViz/src/renderers/equation-renderer.ts)
- Integrate `animateTransformEquation` into the standard rendering flow to smoothly swap LaTeX content.

## Explainer Demo

I will enhance the **"QR & Gram-Schmidt"** example to specifically showcase these transitions, where the projection vector "grows" and the orthogonal vector "slides" into place.

## Verification Plan

### Manual Verification
- Run the QR lesson in **Presentation Mode**.
- Verify that:
  - Vector tips move smoothly to their new positions.
  - The coordinate system pans/zooms smoothly if the range changes.
  - Matrix numbers change with a subtle highlight/fade instead of snapping.
  - Narration text "writes out" instead of appearing instantly.
