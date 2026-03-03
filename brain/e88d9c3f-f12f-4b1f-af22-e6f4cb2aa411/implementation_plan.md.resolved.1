# Dynamic Vertical Expansion for Timetable Slots

This plan addresses the requirement to allow timetable slots to expand vertically to accommodate long text (like module names or venues) while ensuring they remain strictly within their grid boundaries.

## Proposed Changes

### Frontend Timetable Page

#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/timetable/page.tsx)

- Remove `line-clamp` and `overflow-hidden` from the slot content that limits title/module name display.
- Change the grid cell structure in Horizontal View:
    - Instead of a fixed height `h-[110px]`, use `min-h-[110px]` and `h-auto`.
    - Ensure the row (the entire day) expands together if one slot grows.
    - Switch from `absolute` positioning for slots to a flow-based or flex-based approach within the cell if possible, OR use `relative` with `min-height` that matches the span.
- Adjust vertical layout:
    - For the Vertical View, ensuring that if a slot expands, it doesn't overlap the next hour label. This is trickier with absolute positioning. I may need to use `subgrid` or a nested flex layout for the hours.

## Verification Plan

### Manual Verification
1. Add a module with an extremely long name or venue.
2. Verify that the slot expands vertically.
3. Confirm that adjacent slots in the same row/column adjust correctly.
4. Verify that the slot does NOT bleed into the next day or time zone.
