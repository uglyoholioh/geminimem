# Module Hub Filtering & Layout Refinement Plan

This plan addresses the issue of hidden modules appearing in the Module Hub and provides a way for users to manage their displayed modules. It also optimizes the dashboard's vertical space.

## Proposed Changes

### Dashboard Page ([page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/page.tsx))

#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/page.tsx)
- **Filtering Logic**: 
    - Update the `Course` mapping in the Module Hub to filter courses by `is_active === true`.
- **Visibility Management**:
    - Add a "Manage View" overlay/modal that lists all fetched courses with checkboxes or toggles.
    - Implement a local state update and API call to persist the `is_active` status when toggled.
- **Layout Expansion**:
    - Increase the main container height from `600px` to `700px` (or dynamic flex) to fill the bottom space.
    - Adjust the right sidebar stack heights proportionally.

### Backend API ([routers/courses.py] (if needed))
- Ensure the `PATCH /courses/{id}` endpoint correctly updates the `is_active` field (if not already implemented).

## Verification Plan

### Manual Verification
- Verify that only "active" modules appear in the Module Hub initially.
- Open the management overlay and toggle a module's visibility; confirm it appears/disappears from the hub.
- Visually inspect the bottom of the dashboard to ensure the vertical space is filled effectively.
