# Synchronized Step-by-Step Highlighting

This plan outlines the changes needed to support synchronized highlighting across narration text, mathematical equations, and graphical visualizations (graphs, vectors). This will allow for a narrative-driven visualization where specific elements (e.g., a "coefficient") can be highlighted in both text and the visualization simultaneously.

## Proposed Changes

### Core Models & State

#### [MODIFY] [mobject.ts](file:///Users/oli/Desktop/lectvideoanalyser/MathViz/src/core/mobject.ts)
- Add `id` to the function objects in `MGraph.functions` so they can be targeted for highlighting.
- Add `highlights?: Record<string, string>` to `MObjectBase` (or specifically to `MGraph` and `MVector` if broader support isn't needed yet).
- Add `highlightId?: string` to `MGraph` functions and `MVector` to allow global mapping.

#### [MODIFY] [scene.ts](file:///Users/oli/Desktop/lectvideoanalyser/MathViz/src/core/scene.ts)
- Update the `Beat` interface to include an optional `highlights?: Record<string, string>` field.
- Update `resolveStateAtBeat` to propagate these highlights to the objects.

### Renderers

#### [MODIFY] [text-renderer.ts](file:///Users/oli/Desktop/lectvideoanalyser/MathViz/src/renderers/text-renderer.ts)
- Enhance `renderText` to support `{{id:content}}` syntax, similar to how equations work.
- Add a `highlights` parameter to `renderText` to resolve these IDs to colors.

#### [MODIFY] [canvas-renderer.ts](file:///Users/oli/Desktop/lectvideoanalyser/MathViz/src/renderers/canvas-renderer.ts)
- Update `drawGraphFunctions` and `drawVectorObj` to check if a function/vector has a `highlightId` that matches a color in the object's (or beat's) `highlights` map.
- If highlighted, draw with a thicker line or a subtle outer glow (or just the specified color if not already set).

#### [MODIFY] [app.ts](file:///Users/oli/Desktop/lectvideoanalyser/MathViz/src/app.ts)
- Update the narration rendering (in both `renderDocument` and `renderPresentation`) to use the new highlighted text rendering logic.
- Ensure `beat.highlights` are passed down to all sub-renderers (Equation, Text, Canvas).

### Utilities

#### [MODIFY] [katex-helpers.ts](file:///Users/oli/Desktop/lectvideoanalyser/MathViz/src/utils/katex-helpers.ts)
- Ensure the `{{id:content}}` parser is robust and can be reused for plain text if possible, or just used in both locations.

## New Feature Demo

I will add a new example scene called "Synchronized Highlights" that demonstrates:
1. Identifying a coefficient in an equation (highlighting it in Blue).
2. Showing how that coefficient affects the slope of a line in the graph (highlighting the slope in Blue).
3. Linking the narration text to both using the same color.

## Verification Plan

### Automated Tests
- N/A (Visual UI component)

### Manual Verification
- Render the "Synchronized Highlights" example.
- Verify that the narration text `{{id:text}}` is colored correctly.
- Verify that the equation `{{id:math}}` is colored correctly and matches the text.
- Verify that the graph line/vector with the same `highlightId` reflects the color or a highlight state.
- Test in both "Document" and "Presentation" modes.
