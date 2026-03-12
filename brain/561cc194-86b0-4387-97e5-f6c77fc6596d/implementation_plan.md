# Removing Auto-Task Creation

The user reported that the auto-creation of tasks from Canvas assignments and announcements is creating too many tasks and wants it removed.

## Proposed Changes

### Backend Services

#### [MODIFY] [canvas_sync.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/canvas_sync.py)
- Remove the block in `sync_assignments` that auto-creates tasks based on the `auto_tasks_from_assignments` setting.
- Remove the call to `extract_tasks_from_announcement` in `sync_announcements`.
- Remove the `extract_tasks_from_announcement` function entirely.
- Remove unused imports (`ai_service`, `json`, `BeautifulSoup` if only used here).

## Verification Plan

### Manual Verification
- Trigger a Canvas sync for assignments and verify no new tasks are created.
- Trigger a Canvas sync for announcements and verify no tasks are extracted.
