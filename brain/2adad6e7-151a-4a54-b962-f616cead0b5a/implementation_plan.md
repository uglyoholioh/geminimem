# Visual Polish & Style Consistency Plan

This plan aims to elevate the visual experience of CraftCanvas, making it feel more "premium and polished" through consistent design tokens, layout optimizations, and subtle micro-animations.

## Proposed Changes

### Core Design System
- **[MODIFY] [globals.css](file:///Users/oli/Desktop/CraftCanvas/frontend/app/globals.css)**: 
    - Refine color palette with more curated, harmonious shades.
    - Enhance glassmorphism effects (adjust blur/opacity).
    - Add global utility for "premium" interactions (e.g., smoother transitions, subtle scale effects).
    - Standardize spacing and border-radius tokens.

### Dashboard & Layout
- **[MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/page.tsx)**:
    - Tighten up the layout to feel more intentional and less "grid-heavy".
    - Improve the "Up Next" bar's visual hierarchy.
- **[MODIFY] [DashboardHeader.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/DashboardHeader.tsx)**: 
    - Polish typography and layout of header stats.
- **[MODIFY] [WidgetShell.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/WidgetShell.tsx)**:
    - Standardize widget card appearance (consistent shadows, borders, and padding).

### Interactions & Animations
- **[MODIFY] [various components]**:
    - Implement `framer-motion` for entry animations and state transitions.
    - Add hover effects that feel "alive" (subtle shifts in background color or slight lifts).

## Observations & Suggestions

| Area | Observation | Suggestion |
| :--- | :--- | :--- |
| **Typography** | Multiple fonts (`Inter`, `DM Mono`, `Instrument Serif`) are present but could be used more selectively to create hierarchy. | Use `Instrument Serif` strictly for large headers/display text; `Inter` for UI; `DM Mono` for metadata/code. |
| **Colors** | Base colors are good, but some accents might feel a bit too "standard". | Use more nuanced HSL values for accents to give them a custom feel. |
| **Layout** | The dashboard is very feature-rich, which can feel cluttered. | Use a clear "Bento Grid" style with varying card sizes to create visual interest and guide the eye. |
| **Polish** | Many elements have transitions, but they aren't all synchronized. | Create a set of shared `framer-motion` variants for consistent component behavior. |

## Verification Plan

### Manual Verification
- **Visual Inspection**: Cross-browser (Chrome/Safari) check of the dashboard layout.
- **Interactivity Test**: Ensure all hover states and animations feel smooth and responsive.
- **Theme Switching**: Verify that all schemes (Midnight, Forest, Sunset, Dark) maintain readability and premium feel.
