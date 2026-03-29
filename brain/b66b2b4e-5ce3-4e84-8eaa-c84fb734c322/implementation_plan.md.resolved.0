# Implementation Plan - Migrating MathViz to manim-web

The goal is to replace the custom, ad-hoc animation logic in MathViz with `manim-web`, a TypeScript port of the Manim animation engine. This will provide a more robust, cinematic, and professional-grade animation system while maintaining the existing "lesson" and "document" structure.

## User Review Required

> [!IMPORTANT]
> **Shift to Canvas/WebGL**: `manim-web` renders to a Canvas using WebGL (Three.js). This means the math visualizations (matrices, equations, graphs) will now be part of a unified canvas rather than individual DOM elements for the visualizations themselves. Narration and layout will still use HTML/CSS.

> [!WARNING]
> **Breaking Change**: The current `MObject` and `SceneSpec` formats will be updated to better align with `manim-web`'s capabilities. Existing custom renderers (MatrixRenderer, EquationRenderer, etc.) will be refactored into `manim-web` compatible components or "Scenes".

## Proposed Changes

### 1. Dependency Updates
- **[MODIFY] [package.json](file:///Users/oli/Desktop/lectvideoanalyser/MathViz/package.json)**: Add `manim-web` and `three`.

### 2. Core Engine Refactor
- **[NEW] [manim-controller.ts](file:///Users/oli/Desktop/lectvideoanalyser/MathViz/src/core/manim-controller.ts)**: A new controller that manages `manim-web` scenes. It will handle the lifecycle of the `Scene` object and mapping `SceneSpec` beats to `manim-web` animations.
- **[MODIFY] [app.ts](file:///Users/oli/Desktop/lectvideoanalyser/MathViz/src/app.ts)**: Update the `App` class to use the new `ManimController` for rendering visualizations. Replace the "beats" document view to embed `manim-web` canvases.

### 3. Visual Components
- **[NEW] [manim-renderers.ts](file:///Users/oli/Desktop/lectvideoanalyser/MathViz/src/renderers/manim-renderers.ts)**: Helper functions to create `manim-web` objects (Mobjects) for matrices, equations, and graphs.
- **[DELETE] [canvas-renderer.ts](file:///Users/oli/Desktop/lectvideoanalyser/MathViz/src/renderers/canvas-renderer.ts)**: (Optional, eventually) Most of its logic will be superseded by `manim-web`.

### 4. Design & Aesthetics
- Implement a "Studio" look and feel for the lesson interface.
- Use `manim-web`'s built-in `Create`, `Write`, `Transform`, and `FadeIn` animations for that "premium" feel.
- Maintain the sleek dark mode and typography.

## Open Questions

- **Interactivity**: `manim-web` supports interactivity. Do you want to keep the current "draggable point" and "slider" controls within the canvas, or keep them as HTML overlays?
- **LaTeX Rendering**: `manim-web` uses KaTeX internally. Are there specific complex LaTeX structures you need to ensure are supported?

## Verification Plan

### Automated Tests
- `npm run dev` to verify the new engine loads.
- Create a test scene with a `Circle` and `Square` transforming to verify `manim-web` is working.

### Manual Verification
- Verify the "QR & Gram-Schmidt" example renders with fluid animations.
- Check responsiveness of the canvas within the document layout.
