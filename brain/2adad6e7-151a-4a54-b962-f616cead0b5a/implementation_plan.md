# Visual Polish & Style Consistency Plan

This plan aims to elevate the visual experience of CraftCanvas, making it feel more "premium and polished" through consistent design tokens, layout optimizations, and subtle micro-animations.

## Proposed Changes

### Core Design System
- **[MODIFY] [globals.css](file:///Users/oli/Desktop/CraftCanvas/frontend/app/globals.css)**: 
    - Refine color palette with more curated, harmonious shades.
    - Enhance glassmorphism effects (adjust blur/opacity).
    - Add global utility for "premium" interactions (e.g., smoother transitions, subtle scale effects).
    - Standardize spacing and border-radius tokens.

### Global Navigation & Layout
- **[MODIFY] [Sidebar.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/layout/Sidebar.tsx)**:
    - Implement a "premium glass" effect with a subtle border-glow and refined backdrop-blur.
    - Upgrade tooltips to use `backdrop-filter: blur()` and high-end typography.
    - Synchronize popover animations with the rest of the app's motion language.
- **[NEW] [PageTransitions.tsx]**: (or modification to existing layout)
    - Add global `framer-motion` route transitions for a "fluid" app experience.

### Specialized Modes
- **[MODIFY] [app/focus/page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/focus/page.tsx)**:
    - Standardize Clock Typography: Use `Instrument Serif` for "Minimal" and "Glow" styles.
    - Synchronize the Customization Modal with the Dashboard's aesthetic.
    - Polish the Focus Task List to match the updated `WidgetShell` aesthetics.
- **[MODIFY] [app/planner/page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/planner/page.tsx)**:
    - Refine the study grid with lighter, more intentional lines.
    - Elevate the "Study Planner" header to match the Dashboard's premium feel.
    - Polish the "Living Planner" side panel with better depth and spacing.

### Media & Polish
- **[MODIFY] [SpotifyPlayer.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/SpotifyPlayer.tsx)**:
    - Refine the non-vinyl visualizers (Bars, Rings, Waveform) for smoother responsiveness.
    - Standardize typography on the player (use `Instrument Serif` for track titles).
- **[MODIFY] [various components]**:
    - Ensure consistent use of `rounded-2xl` for a soft, modern feel app-wide.
    - Implement subtle "inner-shadows" for depth on interactive cards.

## Observations & Suggestions

| Area | Observation | Suggestion |
| :--- | :--- | :--- |
| **Typography** | Multiple fonts (`Inter`, `DM Mono`, `Instrument Serif`) are present but could be used more selectively to create hierarchy. | Use `Instrument Serif` strictly for large headers/display text; `Inter` for UI; `DM Mono` for metadata/code. |
| **Colors** | Base colors are good, but some accents might feel a bit too "standard". | Use more nuanced HSL values for accents to give them a custom feel. |
| **Navigation** | The Sidebar feels quite "utility" centered. | Add subtle motion feedback when switching routes and a more "glass-morphic" active state. |
| **Continuity** | Switching between Focus and Dashboard feels like entering a different app. | Unify the design language of customization panels and task representations across all pages. |

## Verification Plan

### Manual Verification
- **Visual Inspection**: Cross-browser (Chrome/Safari) check of all main pages.
- **Interactivity Test**: Ensure all hover states and animations feel smooth and responsive.
- **Theme Switching**: Verify that all schemes (Midnight, Forest, Sunset, Dark) maintain readability and premium feel across all modules.
- **Navigation Flow**: Verify that route transitions are smooth and don't cause layout shifts.
