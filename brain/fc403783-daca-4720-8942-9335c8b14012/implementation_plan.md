# Correct Dashboard Restoration Implementation Plan

The previous restoration attempt incorrectly included the full-width Timetable at the bottom of the dashboard. This plan aims to restore the dashboard to its true "original" state: a single-column layout for the AI Brief/Chat, and a two-column grid for "Today's Agenda" (left) and "Academic Track" (right), with no full-width timetable at the bottom.

## Proposed Changes

### Dashboard Page ([page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/page.tsx))

#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/page.tsx)
- **Remove Timetable**: Eliminate the `Academic Timetable` section at the bottom of the page.
- **Restore Grid Layout**:
    - Ensure the "Today's Agenda" and "Academic Track" (Modules) are correctly placed in a `lg:grid-cols-[1fr,360px]` grid.
    - Verify the "Academic Track" (Modules) grid is a 2-column small card layout as it was originally designed.
- **Header Cleanup**: Final verification of the header and greeting logic to ensure no tab remnants exist.

## Verification Plan

### Manual Verification
- Verify that the dashboard is a single page without tabs.
- Verify that "Today's Agenda" and "Academic Track" are side-by-side on large screens.
- Confirm that the full-width weekly timetable is removed from the dashboard (it remains accessible via the `/timetable` page in the sidebar).
- Confirm the layout matches the user's expectation of the "original" dashboard.
