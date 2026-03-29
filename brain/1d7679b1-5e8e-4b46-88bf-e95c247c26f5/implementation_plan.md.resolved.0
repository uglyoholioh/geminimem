# Fix Visualization and Canvas Rendering Issues

The project is currently experiencing several issues with mathematical visualizations:
1. **Canvas Duplication**: Every time a step is rendered, a new canvas element is appended to the UI without properly cleaning up or updating the existing one.
2. **Hardcoded IDs**: The presentation logic hardcodes the coordinate system ID as `'cs'`, which breaks visualizations that use custom IDs (like `sys`, `g1`, etc.).
3. **State Synchronization**: Updates to the visualization state are not correctly reaching the active renderer because of the ID mismatch and redundant object creation.

## User Review Required

> [!IMPORTANT]
> This fix changes how canvases are managed and updated. Ensure that user-defined coordinate system IDs are consistent within a scene to allow smooth transitions.

## Proposed Changes

### Core Logic & App Management

#### [MODIFY] [app.ts](file:///Users/oli/Desktop/lectvideoanalyser/MathViz/src/app.ts)
- Update `updatePresentationState` to dynamically find the correct `csId` from the current state instead of hardcoding `'cs'`.
- Refactor `renderBaseState` to prevent appending duplicate canvases. It should check if a canvas wrapper already exists and either update it or clear it properly.
- Ensure `updateCanvasObjects` is called with the correct `csId`.

### Renderers

#### [MODIFY] [canvas-renderer.ts](file:///Users/oli/Desktop/lectvideoanalyser/MathViz/src/renderers/canvas-renderer.ts)
- Update `createInteractiveCanvas` to check for an existing canvas in the container. If it exists and has the same `csId`, update its state instead of creating a new one.
- Improve the cleanup logic in `destroyCanvas`.
- Ensure the render loop correctly interpolates between states even when the canvas is reused.

---

## Open Questions

- Should I implement a global "current active CS" to simplify the ID tracking, or should I continue supporting multiple coordinate systems in the same view (though currently only one is typically used per beat)?

## Verification Plan

### Automated Tests
- I will use the browser tool to verify that:
    1. Only one canvas element exists in the DOM at any time during presentation.
    2. Visualizations with custom IDs (like the "System of Equations" example) render correctly.
    3. Transitions between steps remain smooth and animate the objects as expected.

### Manual Verification
- Verify that dragging vectors and sliders still works correctly after the refactoring.
- Check that 3D visualizations still function and don't conflict with 2D ones.
