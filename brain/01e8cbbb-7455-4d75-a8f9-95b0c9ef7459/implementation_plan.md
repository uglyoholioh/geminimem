# Fix Meeting Finder Inconsistencies

The meeting finder currently has two major issues:
1. **Day-of-Week Mismatch**: The backend uses Python's `weekday()` (0=Monday, 6=Sunday), while the frontend uses `date-fns` (0=Sunday, 6=Saturday) for date calculations, but then maps them back to a 0=Monday array for display. This leads to gaps or misaligned data.
2. **Date Range Visibility**: The `AvailabilityGrid` logic for determining if a day is in range is flawed when the meeting spans multiple weeks or doesn't start on a Monday.

## Proposed Changes

### [Backend] [Component: Meetings Router]

#### [MODIFY] [meetings.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/meetings.py)
- Clarify in comments that `day_of_week` uses 0=Monday.
- Add validation that `time_range_start` < `time_range_end`.

### [Frontend] [Component: Meetings Components]

#### [MODIFY] [AvailabilityGrid.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/meetings/AvailabilityGrid.tsx)
- Re-index `date-fns` `getDay()` results to match the 0=Monday convention (Python ISO).
- Fix `visibleDays` logic to correctly iterate over exactly the relevant dates within the `dateRange`.
- Ensure consistent color mapping and legend display.

## Verification Plan

### Automated Tests
- Run `backend/repro_meeting.py` to verify backend consistency.
- Add a new frontend unit test `frontend/components/meetings/__tests__/AvailabilityGrid.test.tsx` (if environment supports it) or manual verification via browser.

### Manual Verification
1. Create a meeting spanning a weekend.
2. Join as a participant and add availability.
3. Verify that the slots saved correspond to the correct days on the grid.
4. Verify that "Locked View" correctly prevents edits.
