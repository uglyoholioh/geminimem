# Dashboard Modules Access Implementation Plan

The goal is to make it easy for users to access their modules directly from the dashboard. This will be achieved by adding a new "Your Modules" widget in the right column of the dashboard.

## Proposed Changes

### [Component Name] Dashboard Page (`frontend/app/page.tsx`)

#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/page.tsx)
- Add `Course` interface.
- Fetch courses from `/courses?active_only=true` in the `load` function.
- Add a new "Your Modules" section in the right column (grid).
- The section will display a list of active modules as clickable cards or a horizontal/grid list.
- Each module will show its code and name, using the module's color for visual reference.

## Verification Plan

### Manual Verification
- Verify that the "Your Modules" section appears on the dashboard.
- Verify that only active modules are shown.
- Verify that clicking a module navigates to the module's detail page (`/courses/[id]`).
- Verify that the design feels premium and matches the rest of the dashboard.
