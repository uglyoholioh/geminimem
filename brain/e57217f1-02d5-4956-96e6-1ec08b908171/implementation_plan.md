# UI Refinement Plan - "Premium Polish"

This plan outlines a series of UI/UX refinements across the CraftCanvas web application to elevate its aesthetic to a "premium" level. The focus is on consistency, depth, and micro-interactions.

## Proposed Changes

### [Dashboard Component Refinement]

#### [MODIFY] [WidgetShell.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/WidgetShell.tsx)
- Update the `glass-card` class usage to be more consistent.
- Enhance the header with a more subtle background and better spacing.
- Improve the resize and remove buttons with better hover states and transitions.
- Add a subtle inner glow or border gradient to the card to give it more depth.

#### [MODIFY] [ModuleHub.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/ModuleHub.tsx)
- Refine the module cards to use the `glass-card` style.
- Replace the simple top-border with a more integrated accent element (e.g., a subtle glow or a vertical bar).
- Improve the "Due" label styling with better icons and colors.
- Add hover effects like a slight lift (`-translate-y-1`) and increased shadow.

#### [MODIFY] [CommandCenter.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/CommandCenter.tsx)
- Adjust the layout to reduce "empty space" feel.
- Refine the chat input area to feel more integrated (glassmorphic input).
- Update suggestions buttons to use a more premium, pill-shaped design with subtle borders.

### [Global Styling & Foundations]

#### [MODIFY] [globals.css](file:///Users/oli/Desktop/CraftCanvas/frontend/app/globals.css)
- Refine the `.glass-card` and `.glass-morphism` utilities for better depth and consistency.
- Add new utility classes for premium hover effects (e.g., `.hover-lift`, `.hover-glow`).
- Audit and potentially adjust font weights for headers to improve readability and "premium" feel.
- Improve the scrollbar styling to be even more discreet.

### [Page-Specific Polishing]

#### [MODIFY] [Courses Page](file:///Users/oli/Desktop/CraftCanvas/frontend/app/courses/page.tsx)
- Bring the "Stats Cards" in line with the new premium card style.
- Standardize course cards to match the Dashboard's Module Hub cards.
- Add subtle animations when filtering or searching courses.

#### [MODIFY] [Focus Page](file:///Users/oli/Desktop/CraftCanvas/frontend/app/focus/page.tsx)
- Update the main timer card to use glassmorphism with a stronger backdrop blur.
- Refine the "Focusing On" section to feel more complete and less like a "placeholder."
- Add subtle ambient animations to the background or timer during active focus sessions.

## Verification Plan

### Manual Verification
- **Visual Audit**: Use the browser tool to navigate to the Dashboard, Courses, and Focus pages in both Light and Dark modes to verify the new styles.
- **Interactions**: Manually verify all hover states, transitions, and animations feel smooth and responsive.
- **Responsive Check**: Ensure the new styles don't break the layout on different screen sizes (using browser tool to resize).

### Automated Tests
- No new automated tests are planned as these are primarily visual changes. Existing E2E tests should be run to ensure no functionality is broken.
- Command: `npm test` or `npx playwright test` (if applicable).
