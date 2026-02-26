# Goal Description
Refine the dashboard layout to emphasize the Daily Brief by making its window bigger and rearranging other elements for better flow.

## Proposed Changes

### Frontend Layout
- [MODIFY] `frontend/app/page.tsx`
  - Rearrange grid items:
    - **Column 1 (Left)**: Daily Brief & Chat (expanded to take more vertical/flex space).
    - **Column 2 (Right)**: Today's Schedule, Tasks, and Assignments.
    - **Bottom or Inline**: Announcements (optimized to fit more compact or separate section).
  - Adjust `flex` properties to ensure Daily Brief feels like the "command center".

## Verification Plan

### Manual Verification
1. Open the dashboard and verify the Daily Brief is significantly larger.
2. Ensure the right column (Schedule, Tasks, Assignments) is readable and correctly stacked.
3. Check responsiveness on different screen sizes.

