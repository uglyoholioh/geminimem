# Fix Backlog Type Error in Study Planner

The Study Planner (`/planner`) is currently crashing because it attempts to call `.map()` on the `backlog` state, which is being set to an object `{"tasks": [...]}` instead of the array itself. Additionally, the AI-generated study blocks ("ghost slots") are not appearing because they are also being incorrectly extracted from the response.

## Proposed Changes

### [Component] Frontend Planner Page

#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/planner/page.tsx)

- Update `fetchPlannerData` to correctly extract the `tasks` array from the `/tasks` response.
- Update `fetchPlannerData` to correctly handle the `/timetable/ai-plan` response, which returns a list directly.

```diff
-            setBacklog(tasksRes || [])
-            setGhostSlots(aiRes.suggested_slots || [])
+            setBacklog(tasksRes.tasks || [])
+            setGhostSlots(Array.isArray(aiRes) ? aiRes : (aiRes.suggested_slots || []))
```
*(Note: Adding a check for `Array.isArray` for robustness, though the backend currently returns a list.)*

## Verification Plan

### Manual Verification
1. **Navigate to Study Planner**: Go to `/planner` in the application.
2. **Verify Backlog**: Ensure the "Backlog" sidebar displays items correctly and no "Something went wrong" error appears.
3. **Verify Ghost Slots**: If there are upcoming assignments, check if "Suggested Study Block" items (ghost slots) appear in the timetable grid.
4. **Test Sorting Gauntlet**: Click "Sorting Gauntlet" to ensure it opens with the correct task data.
