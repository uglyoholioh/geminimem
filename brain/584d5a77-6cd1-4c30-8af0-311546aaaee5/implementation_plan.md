# Dynamic Timetable Hours

The goal is to optimize the timetable view by hiding hours with no classes scheduled across the entire week, shifting the range to fit the user's actual schedule.

## Proposed Changes

### Frontend — [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/timetable/page.tsx)

1. **Remove static `TIME_SLOTS` constant.**
2. **Implement `timeSlots` useMemo** inside `TimetablePage`:
   - Union the hour range `[min(start_time), max(end_time)]` across all slots in the week.
   - Fallback to a standard 8 AM - 6 PM range if no slots are present.
3. **Refine Span Logic**: Update `getColOrRowSpan` and `isSpanned` to round up hours for classes ending with minutes (e.g., 10:30 ends effectively at the 11:00 mark in the hour-based grid).
4. **Update Rendering**: Both horizontal and vertical views will use the dynamic `timeSlots` array.

## Verification Plan

### Manual Verification
1. Open a week with classes (e.g., teaching weeks).
2. Verify the hour range starts at the earliest class and ends at the latest.
3. Check both Horizontal and Vertical view modes.
4. Verify non-teaching weeks still display a reasonable default range in the grid (if shown).
