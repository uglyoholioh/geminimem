# Phase 4: Task Synchronization and Final Polish

This phase focuses on implementing robust task synchronization for Google Tasks and Apple Reminders, integrating it into the background sync cycle, and providing a final polish to the application's remaining features.

## Proposed Changes

### 1. Task Synchronization Implementation
The `TaskSyncService` in `calendar_sync.py` has initial logic that needs to be refined for better status mapping, bidirectional sync, and error handling.

#### [MODIFY] [backend/services/calendar_sync.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/calendar_sync.py)
- Refine `sync_google_tasks`: Improve status mapping between Google (needsAction, completed) and local (inbox, in_progress, completed).
- Refine `sync_apple_reminders`: Improve status mapping and handle task notes/due dates if available in CalDAV.
- Ensure deduplication using `google_tasks_id` and `apple_reminders_id`.

#### [MODIFY] [backend/jobs/scheduler.py](file:///Users/oli/Desktop/CraftCanvas/backend/jobs/scheduler.py)
- Integrate `sync_google_tasks` and `sync_apple_reminders` into the `calendar_sync_job`.

### 2. Frontend Integration
Enable users to trigger task sync and view external tasks.

#### [MODIFY] [frontend/app/settings/page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/settings/page.tsx)
- Add "Sync Tasks" buttons to the Google and Apple integration sections.
- Display last sync status for tasks.

### 3. Final Polish and Cleanup
Address minor technical debts and ensure a consistent user experience.

#### [MODIFY] [backend/schemas/course.py](file:///Users/oli/Desktop/CraftCanvas/backend/schemas/course.py) & [backend/schemas/task.py](file:///Users/oli/Desktop/CraftCanvas/backend/schemas/task.py)
- Fix Pydantic V2 deprecation warnings (`class Config` -> `model_config`).

## Verification Plan

### Automated Tests
- **Task Sync**: Add unit tests for `TaskSyncService` logic, mocking external APIs.
- **Pydantic Models**: Ensure no deprecation warnings are emitted during tests.

### Manual Verification
1. **Google Tasks**: Create a task in Google Tasks and verify it appears in CraftCanvas after sync.
2. **Apple Reminders**: Create a reminder on iPhone/Mac and verify it syncs to CraftCanvas.
3. **Background Sync**: Verify that tasks are automatically synchronized without manual intervention.
