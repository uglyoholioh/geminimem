# Task: Codebase Simplification and Error Reduction

## Backend Simplification
- [x] Consolidate database session functions in `database.py`
- [x] Refactor `get_current_user` in `dependencies.py` to be more robust and less redundant
- [x] Analyze `routers/` for overlapping functionality and consolidate where possible
- [x] Review `services/` for duplicate logic, especially in sync services

## Frontend Simplification
- [x] Review `lib/api.ts` for unified error handling and request logic
- [x] Audit `components/` for duplicate UI patterns and extract common components
- [x] Check `hooks/` and `lib/` for redundant utility functions

## Reliability Improvements
- [x] Standardize error response format across backend routers
- [x] Improve logging for background tasks in `main.py`
- [x] Add basic validation schemas where missing

## Error Logging & Handling Improvements
- [x] Refactor `backend/lib/logging_utils.py` for better structured logging
- [x] Implement global exception handler in `backend/main.py`
- [x] Standardize `HTTPException` across routers
- [x] Enhance frontend error feedback in `frontend/lib/api.ts`
