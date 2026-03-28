# Task: Verify Planner and Sweep Features

## Checklist:
- [x] Navigate to `/planner`
- [x] Login (if needed) - Already logged in as oliverkoh96@gmail.com
- [x] Verify keyword filtering ("Quiz") - **FAILED**: Filter input does not affect the list.
- [x] Verify keyword exclusion ("-Quiz") - **FAILED**: Filter input does not affect the list.
- [x] Verify "Hide Synced" (assignments hidden) - **FAILED**: Toggle works but items remain visible.
- [x] Navigate to `/planner/sweep`
- [x] Verify bulk actions (search -> bulk bar -> "Tmrw" -> toast) - **PARTIAL**: Search/Bar works, but action failed with 500 error.
- [x] Final summary of results

## Observations:
- **Planner Page**: 
    - Header filter ("Filter tasks...") is non-functional.
    - "Hide Synced" button toggles but does not hide synced assignments.
    - "Group by Module" and "Group by Type" buttons are functional.
- **Daily Sweep Page**:
    - Search bar successfully filters items and triggers the "Bulk Action" bar.
    - Bulk Action bar correctly identifies the number of matches (e.g., 6 matches for "Online Video").
    - Clicking "Tmrw" in the bulk bar triggers API calls, but they returned **500 Internal Server Error** for the assignments (IDs 137, 138, 9, 14, 18, 21).
    - An error indicator ("6 Issues") appeared instead of a success toast.
