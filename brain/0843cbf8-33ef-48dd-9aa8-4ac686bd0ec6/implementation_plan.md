# Plan: Fix Newly Introduced Errors

This plan addresses a series of critical errors identified in the backend logs, frontend logs, and browser environment.

## Proposed Changes

### [Backend] Authentication
#### [MODIFY] [auth.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/auth.py)
- Update password length validation to check byte-length (UTF-8) instead of just character count, preventing bcrypt `ValueError`.
- Ensure consistency between login and registration password validation.

### [Backend] Models
#### [MODIFY] [announcement.py](file:///Users/oli/Desktop/CraftCanvas/backend/models/announcement.py)
- Add `course_code: Optional[str] = None` to the `Announcement` model to fix `AttributeError` during brief generation.

#### [MODIFY] [canvas_file.py](file:///Users/oli/Desktop/CraftCanvas/backend/models/canvas_file.py)
- Ensure the model matches the expected DB schema, and investigate a safe way to apply the missing `canvas_web_url` column to the existing database.

### [Backend] Services
#### [MODIFY] [canvas_sync.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/canvas_sync.py)
- Update `sync_all` signature to correctly handle the `background_tasks` argument passed by the scheduler.

### [Frontend] Assets
#### [NEW] [architect.png](file:///Users/oli/Desktop/CraftCanvas/frontend/public/companions/architect.png)
- Add the missing companion icon to resolve the 400 Bad Request error in the browser.

## Verification Plan

### Automated Tests
- Run `pytest` if available for the backend to ensure no regressions in auth flows.
- Trigger a manual Canvas sync via the API (`POST /api/v1/sync/canvas`) and verify logs for success.
- Trigger a manual brief regeneration (`POST /api/v1/brief/regenerate`) and verify the `course_code` error is resolved.

### Manual Verification
- Attempt to register with a long password (72 characters with multi-byte characters like emojis) to verify the fix.
- check the frontend dashboard at `http://localhost:3000/setup` to confirm the Architect icon loads correctly.
