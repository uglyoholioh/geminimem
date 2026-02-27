# Project Restructuring Plan

This plan aims to clean up the project structure, move documentation to a dedicated folder, and organize the backend components to reduce clutter and improve maintainability.

## Proposed Changes

### Documentation & Reference
- [NEW] Create `docs/` folder in the root.
- [MOVE] Move the following files from root to `docs/`:
    - `AI_LAYER.md`
    - `ARCHITECTURE.md`
    - `BEST_PRACTICES.md`
    - `BUILD_PLAN.md`
    - `DATA_SCHEMA.md`
    - `DESIGN_SYSTEM.md`
    - `FEATURES.md`
    - `GEMINI.md`
    - `INTEGRATIONS.md`
    - `OPUS_PLANNING_BRIEF.md`
    - `mockup.html`
    - `ReferenceFiles/` (move the whole directory)

### Backend Cleanup
- [NEW] Create `backend/scripts/` folder.
- [NEW] Create `backend/logs/` folder.
- [MOVE] Move utility scripts from `backend/` to `backend/scripts/`:
    - `repro_meeting.py`
    - `test_brief.py`
    - `verify_craft_update.py`
- [MOVE] Move logs from `backend/` to `backend/logs/` (and update `.gitignore` if needed):
    - `backend.log`
    - `bg.log`
    - `fastapi.log`
    - `out.log`
    - `uvicorn.log`
- [DELETE] Remove junk files in `backend/`:
    - `3`
    - `aux`
    - `ps`
    - `sleep`
- [DELETE] Remove redundant database files in `backend/` (after confirming they are not in use):
    - `app.db`
    - `craft.db`
    - `craft_canvas.db`
    - `database.db`
    - (Wait! `app.db` might be used. I should double check `main.py` or `database.py` again, but `config.py` says `./data/db.sqlite`)

### Data Consolidation
- [DELETE] Remove root `data/` directory (redundant as `backend/data/` is the source of truth).

### Frontend Cleanup
- [DELETE] Remove `frontend/bg.log`.

## Verification Plan

### Automated Tests
- Run backend tests to ensure nothing broke: `cd backend && pytest`
- Run frontend build to ensure imports are still correct: `cd frontend && npm run build` (or just check if it starts)

### Manual Verification
- Check that all moved files are accessible in their new locations.
- Verify that the backend server still starts: `cd backend && uvicorn main:app --port 8000`
- Verify that the workspace docs still work (if the user uses them).
