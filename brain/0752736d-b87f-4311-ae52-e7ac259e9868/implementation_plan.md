# Codebase Simplification and Error Reduction

This plan aims to simplify the codebase by removing redundancy and consolidating logic. This will reduce the surface area for errors and make the project easier to maintain.

## Proposed Changes

### Backend Core
- [MODIFY] [database.py](file:///Users/oli/Desktop/CraftCanvas/backend/database.py): Consolidate `get_session` and `get_session_sync` into a single, robust generator.
- [MODIFY] [dependencies.py](file:///Users/oli/Desktop/CraftCanvas/backend/dependencies.py): Refactor `get_current_user` to reduce duplication in auth methods and improve error reporting.

### Routers & Consolidation
- [MODIFY] [main.py](file:///Users/oli/Desktop/CraftCanvas/backend/main.py): Reorganize router includes and simplify middleware if possible.
- [DELETE] [task_sync.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/task_sync.py): Move task sync endpoints to `sync.py`.
- [MODIFY] [sync.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/sync.py): Incorporate Google/Apple task sync endpoints from `task_sync.py`.

### Frontend
- [MODIFY] [api.ts](file:///Users/oli/Desktop/CraftCanvas/frontend/lib/api.ts): Standardize error handling and ensure timeouts are handled gracefully.

## Verification Plan

### Automated Tests
- Run existing router tests: `pytest backend/tests/test_routers/`
- Run existing service tests: `pytest backend/tests/test_services/`
- Specifically verify auth changes with `pytest backend/tests/test_routers/test_auth.py`

### Manual Verification
1. **Login Flow**: Verify that login/session handling still works correctly after `get_current_user` refactor.
2. **Sync Operations**: Trigger Canvas, Google, and Apple syncs via the UI to ensure consolidation hasn't broken functionality.
3. **Error Feedback**: Simulate a backend error (e.g., stop the database) and verify that the frontend `api.ts` handles it gracefully with a clear error message.
