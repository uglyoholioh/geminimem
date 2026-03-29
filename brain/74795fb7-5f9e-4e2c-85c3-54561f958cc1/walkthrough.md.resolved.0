# Synchronized Mathematical Highlights

I have implemented a new feature that allows for synchronized, narrative-driven mathematical visualizations. This system links textual explanations, symbolic math (LaTeX), and geometric visualizations (graphs, vectors) through a shared highlighting mechanism.

## Key Features

- **Cross-Component Highlighting**: Use the `{{id:content}}` syntax in both narration and math equations to apply highlights based on a global `highlights` map in each scene step (Beat).
- **Visual Synchronization**: Graph functions and vectors now support `highlightId` or can be targeted by `id` to automatically reflect the same highlight color and style (e.g., thicker lines, subtle glow).
- **Unified Style**: A consistent visual language where the same color always refers to the same mathematical entity across all parts of the UI.
- **Improved Narration**: Narration text now supports inline math (`$...$`) and highlights, providing a cohesive "video-like" explanation experience.

## Changes Made

### Core Models & Logic
- **`mobject.ts`**: Expanded `MObjectBase` and `MGraph` to support `highlights` and per-function/vector IDs.
- **`scene.ts`**: Updated the `Beat` structure to support global highlights and enhanced `resolveStateAtBeat` to propagate these highlights to all objects in a step.

### Renderers & Utilities
- **`text-renderer.ts`**: Updated to support the highlight syntax and synchronized colors.
- **`canvas-renderer.ts`**: Enhanced 2D rendering for graphs and vectors to visually reflect the "highlighted" state with increased line width and outer glows.
- **`katex-helpers.ts`**: Added a new `renderHighlightedText` utility to handle the shared highlight/math syntax across the app.
- **`app.ts`**: Orchestrated the passing of highlights from the scene definition to all sub-renderers in both Document and Presentation modes.

## Verification Results

### Visual Demo: Synchronized Highlights
I added a new example called **"Synchronized Highlights"** under the **Features** category.

![Synchronized Highlights Demo](/Users/oli/.gemini/antigravity/brain/74795fb7-5f9e-4e2c-85c3-54561f958cc1/full_page_highlights_1774760009109.png)
*Figure 1: The new example showing linked highlights between text, math, and the graph.*

### Test Summary
- [x] **Narration Highlights**: Verified that `{{m:slope}}` renders in the correct color.
- [x] **Math Synchronization**: Verified that the same term in LaTeX is colored identically.
- [x] **Graph Highlights**: Verified that the line and specific points update their visual style when highlighted.
- [x] **Mode Support**: Both Document (scrollable) and Presentation (step-by-step) modes correctly render the highlights.
