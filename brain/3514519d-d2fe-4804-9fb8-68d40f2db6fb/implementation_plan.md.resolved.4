# Implementation Plan: Manim-Style Animation Overhaul (Revised)

To achieve the "Manim aesthetic," we will focus on three core pillars: **Organic Easing**, **Geometric Creation**, and **Structural Equation Morphing**.

## User Review Required

> [!IMPORTANT]
> The biggest visual shift is the **Structural Equation Morphing**. Instead of equations simple "changing," matching terms will literally travel across the screen to their new positions.

## Proposed Changes

### 🧪 1. Animation Engine (Easing & Sync)

#### [MODIFY] [animations.ts](file:///Users/oli/Desktop/lectvideoanalyser/MathViz/src/core/animations.ts)
- Standardize on **Quintic Easing** (`smooth`): `t => t^3 * (10 - 15t + 6t^2)`.
- Update `animate` to use this by default for a cinematic look.
- Support `lagRatio` in sequences to stagger the appearance of group elements (Manim's `LaggedStart`).

### 🎨 2. Geometric Creation (`Create`)

#### [MODIFY] [canvas-renderer.ts](file:///Users/oli/Desktop/lectvideoanalyser/MathViz/src/renderers/canvas-renderer.ts)
- Enable **Partial Drawing** for Vectors and Lines.
- When an object is added with a `Create` animation, it will "grow" from its origin to its destination rather than just fading in.
- This creates the classic "animated textbook" feel.

### 🖋️ 3. Structural Morphing (`ReplacementTransform`)

#### [MODIFY] [equation-morph-renderer.ts](file:///Users/oli/Desktop/lectvideoanalyser/MathViz/src/renderers/equation-morph-renderer.ts)
- **ID Matching**: Use `\htmlId` from our KaTeX engine to track terms (e.g., $v_1$ in step A matches $v_1$ in step B).
- **Position Tracking**: Use `getBoundingClientRect` to calculate precise screen coordinates for these terms.
- **Motion Animation**: Animate these terms as fixed-position clones that slide from their old to new coordinates, while non-matching parts cross-fade.

### ✨ 4. Visual Polish

#### [MODIFY] [colors.ts](file:///Users/oli/Desktop/lectvideoanalyser/MathViz/src/utils/colors.ts)
- Rebrand with the **Manim Color Palette** (High-contrast Teal, Gold, Blue, and Red).
- Darken the background to a deeper, professional slate.

## Open Questions

> [!NOTE]
> Do you want the "Creation" animation to be the default for all new objects in the presentation, or should it only happen when I use the `add` keyword in the beat spec?

## Verification Plan

### Automated Tests
- None.

### Manual Verification
- **Gram-Schmidt Step 1**: Verify $u_1$ grows into existence (Create).
- **Equation Transition**: Verify that in "u2 = v2 - p", the symbol $v_2$ moves from its previous position in the narration to its new position in the equation.
- **Camera Movement**: Verify that pans/zooms feel "cushioned" and smooth.
