# Implementation Plan - Fix UI Layout and Positioning

The user reported that the controls and UI elements (Overlay, Timeline, Sidebar) are not displaying properly or in the correct positions. Analysis revealed that the primary cause is the absence of Tailwind CSS utility classes in the generated styles, causing elements that should be `absolute` or `relative` to default to `static` positioning.

## User Review Required

> [!IMPORTANT]
> The fix involves adding `@import "tailwindcss";` to the main CSS file. This will enable all Tailwind utility classes (`flex`, `absolute`, `relative`, etc.) across the application. I will also transition some custom theme variables to be more Tailwind 4-compatible.

## Proposed Changes

### Core Styling

#### [MODIFY] [index.css](file:///Users/oli/Desktop/MathLessonVisualiser/src/index.css)
- Add `@import "tailwindcss";` at the top of the file to enable Tailwind 4 processing.
- Ensure that custom `:root` design tokens are preserved but also integrated with Tailwind's `@theme` where appropriate for better utility class interoperability (e.g. colors, transitions).
- Verify that entrance animations for `.overlay-panel` and `.timeline-bar` do not conflict with Tailwind's centering logic.

### Component Positioning Refinement

#### [MODIFY] [Player.tsx](file:///Users/oli/Desktop/MathLessonVisualiser/src/components/Player.tsx)
- Ensure the main container and the canvas area have the correctly inferred layout (e.g. `flex-1` for the canvas).
- Double check that `w-full` and `h-full` are correctly applied to the main wrapper.

## Verification Plan

### Automated/Browser Tests
- Use the browser tool to verify that:
    - `.overlay-panel` has `position: absolute` and is at `top: 24px` (6 units).
    - `.timeline-bar` has `position: absolute`, is centered horizontally, and anchored at the bottom.
    - Backdrop blurs and backgrounds (glassmorphism) are correctly rendered.
    - The custom entrance animations run smoothly without breaking the final position.

### Manual Verification
- View the app in the browser and confirm that the "dark editorial" aesthetic is restored with properly floating elements.
