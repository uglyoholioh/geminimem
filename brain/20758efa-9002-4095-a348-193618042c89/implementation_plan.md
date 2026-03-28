# Planner Enhancements: Categorization & Advanced Sweep

Improve the space efficiency and utility of the Planner by adding optional grouping (By Module/Type) and enhancing the Daily Sweep experience.

## Proposed Changes

### Frontend: Planner Page
#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/planner/page.tsx)
- Add `groupBy` state (`'none' | 'module' | 'type'`).
- Implement a floating or header-integrated toggle for `groupBy`.
- Refactor `todayTimeline` and `horizonGroups` rendering to support grouping headers when enabled.
- Ensure "Needs Decision" section remains distinct.

### Frontend: Daily Sweep
#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/planner/sweep/page.tsx)
- **Keyboard Shortcuts**: Add `window` event listener for keys `1` (Done), `2` (Today), `3` (Tomorrow), `4` (Next Week), `5` (Someday), `6` (Delete).
- **Advanced Bins**:
    - Increase the action buttons from 2 to 6.
    - Add "Today", "Next Week", "Someday" (Clear schedule), and "Delete" (Delete task).
    - Update the visual deck to be more informative about the chosen bin.
- **API Integration**: Connect new bins to appropriate `PUT` and `DELETE` endpoints.

### Backend: Sweep & Tasks
#### [MODIFY] [sweep.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/sweep.py)
- (Optional) Add a bulk `reschedule` endpoint if needed, otherwise use existing `/tasks/{id}`.

## Verification Plan

### Automated Tests
- Run `npm run build` to ensure no regressions.
- Run `vitest` to ensure component logic is sound.

### Manual Verification
- **Categorization**: Toggle between "None", "Module", and "Type" on the Planner page. Verify that items group correctly.
- **Daily Sweep**:
    - Open Sweep with overdue items.
    - Use keys 1-6 to process cards.
    - Verify that tasks are moved to the correct dates/states in the database.
    - Verify that "Delete" actually removes the task.

## Phase 3: Advanced Filtering & Bulk Actions

Enhance the Planner and Daily Sweep to handle high-volume, automated tasks from Canvas with pattern-based filtering and batch processing.

### Proposed Changes

#### [MODIFY] [types.ts](file:///Users/oli/Desktop/CraftCanvas/frontend/lib/types.ts)
- Add `source?: string` to `Task` interface (canvas | telegram | craft | web).

#### [MODIFY] [planner/page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/planner/page.tsx)
- Add `searchQuery` and `hideSynced` state.
- Update grouping and filtering logic to support **Keyword Search** (Title/CourseCode) and **Exclusion** (e.g. `-Lab`).
- Add a Search/Filter component with a 'Hide Synced' toggle.

#### [MODIFY] [planner/sweep/page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/planner/sweep/page.tsx)
- Add `searchQuery` state and filter bar.
- Implement **Bulk Actions**: When the deck is filtered, provide a "Process All Matches" button that applies the selected bin action to ALL remaining items in the filtered set.

## Verification Plan

### Manual Verification
- **Fuzzy Search**: Type keywords like "Lab" or "Quiz" and verify filtering.
- **Exclusion**: Type "-Tutorial" and verify items are hidden.
- **Bulk Sweep**: Filter by a keyword, then use a Bulk Action to move all matches at once.
