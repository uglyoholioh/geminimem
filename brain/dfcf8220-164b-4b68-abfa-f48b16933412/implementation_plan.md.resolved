# Implementation Plan - Multiple Planner Views

We will implement "Linear Flow", "Focus Cards", and "Dynamic List" as switchable viewing options within the Study Planner. This allows users to choose the level of detail and visual density they prefer.

## Proposed Changes

### [Frontend]
Modify the study planner to support multiple view modes.

#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/planner/page.tsx)
- Introduce `viewMode` state (defaulting to 'grid' or a saved preference).
- Add a "View Switcher" component to the header.
- Conditionally render the main content based on `viewMode`.

#### [NEW] [LinearFlowView.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/planner/LinearFlowView.tsx)
- A streamlined vertical timeline for a single day.
- Focuses on chronological flow with high legibility.

#### [NEW] [FocusCardsView.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/planner/FocusCardsView.tsx)
- Bento-style grouping into Morning, Afternoon, and Evening buckets.
- Reduces precision-based anxiety.

#### [NEW] [DynamicListView.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/planner/DynamicListView.tsx)
- A high-contrast task list with a horizontal date picker.
- Moves the companion to a collapsible or floating state.

## Verification Plan

### Manual Verification
- Navigate to the Study Planner.
- Switch between the four views (Grid, Linear, Focus, List) using the new header control.
- Verify that data (tasks, classes) is correctly represented and synced across all views.
- Ensure the `viewMode` persists across page reloads using `localStorage`.
