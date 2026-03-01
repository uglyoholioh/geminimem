# CraftCanvas Implementation Plan - UI/UX Audit and Enhancements

This plan outlines the steps to visually audit the CraftCanvas frontend, identify areas for improvement in aesthetics and structure, and implement a more premium, dynamic user interface.

## User Review Required

> [!IMPORTANT]
> I will be performing a visual audit using the browser tool. I will propose specific UI/UX changes after the audit.
> Significant restructuring of the dashboard or main navigation will be presented for approval before implementation.

## Proposed Changes

### [Phase 1: Visual Audit & Research] [DONE]
- Captured screenshots and analyzed Dashboard, Tasks, Courses, Grades, and Settings.
- Identified issues: low information density in Command Center, wide list items in Tasks, oversized course cards with disconnected actions, and sparse empty states.

### [Phase 2: Core Design System & Aesthetics]
- **[MODIFY] [globals.css](file:///Users/oli/Desktop/CraftCanvas/frontend/app/globals.css)**
  - Enhance glassmorphism: Add `backdrop-blur-md`, `bg-white/40` (light) and `bg-black/40` (dark) with subtle borders.
  - Refine color palette: Introduce more vibrant HSL-based accent variations for "premium" feel.
  - Typography: Adjust font sizes and weights for better hierarchy (e.g., smaller fonts for metadata, bolder for primary actions).

### [Phase 3: Component & Layout Refinement]
- **[MODIFY] [ModuleHub.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/ModuleHub.tsx)**
  - Implement 2-column grid layout for higher density.
  - Simplify card header: focus on module codes as primary identifiers.
  - Enhance hover state: smoother transitions and clearer action reveal.
- **[MODIFY] [AgendaTimeline.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/AgendaTimeline.tsx)**
  - Increase vertical density: reduce padding between agenda items.
  - Improve visual hierarchy of time and event title.
- **[MODIFY] [DashboardHeader.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/DashboardHeader.tsx)**
  - Tighten spacing between greeting and stat badges.
  - Enhance stat badges with subtle borders and glassmorphism.

### [Phase 4: Dashboard & Page Polish]
- **[MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/page.tsx)**
  - Optimize "Command Center" (left section) for better information utilization.
  - Increase vertical utilization of the screen to eliminate empty space.
- **[MODIFY] [tasks/page.tsx] (and others)**
  - Optimize list item width and padding for better density.
- **[MODIFY] [Logo.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/Logo.tsx)**
  - Ensure the SVG logo is sharp and fits the premium aesthetic.


## Verification Plan

### Automated Tests
- Run existing Playwright E2E tests: `npx playwright test`.
- Check for accessibility (aria-labels, contrast) using browser audit tools.

### Manual Verification
- Visual inspection of all screens in the browser subagent.
- Verify "Wow factor" - does it feel premium and alive?
- Check responsiveness on different viewport sizes.
