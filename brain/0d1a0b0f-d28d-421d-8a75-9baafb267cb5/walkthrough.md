# CraftCanvas Architecture Remediation

This walkthrough documents the technical debt reduction and infrastructure improvements made to the CraftCanvas application based on the architectural evaluation.

## Changes Made

### 1. Priority Bug Fixes
- **Middleware Scoping**: Moved the Request-ID middleware outside of the `global_exception_handler` function in `main.py` so that it actually executes on every request.
- **Undefined Variables**: Fixed a bug in `get_courses` where a `statement` variable was referenced before assignment, replacing it with a correct `SQLModel` select query.
- **Orphaned Jobs**: Registered the `calendar_sync_job` in `scheduler.py` so that calendar syncing actually occurs as designed.

### 2. Deconstructing the `ai_tools.py` Monolith
The 1,926-line `ai_tools.py` file was extremely difficult to navigate. It has been successfully split into a modular package:
- `backend/services/ai_tools/courses.py`
- `backend/services/ai_tools/tasks.py`
- `backend/services/ai_tools/materials.py`
- `backend/services/ai_tools/misc.py`
- `backend/services/ai_tools/helpers.py`

The new `__init__.py` file aggregates all tools into the `TOOL_DEFINITIONS` list, preserving seamless compatibility with `ai_service.py` without requiring changes to how tools are loaded.

### 3. Deconstructing the `page.tsx` Dashboard
The 730-line Next.js dashboard had too many intertwined state variables and side effects. We extracted all data fetching and state management into three dedicated custom hooks:
- **`useDashboardData`**: Manages all core data (tasks, assignments, courses, brief, triage, fluid suggestions).
- **`useDashboardTimetable`**: Manages today's timetable slots and the multi-day lookahead logic.
- **`useDashboardChat`**: Manages the SSE streaming connection, chat history, and chat UI state.

`page.tsx` is now significantly smaller and strictly focused on composing UI components.

### 4. Database Migrations via Alembic
We introduced Alembic to manage database schema changes safely moving forward:
- Initialized Alembic in the backend.
- Configured `alembic/env.py` to correctly import and read `SQLModel.metadata` from all 24 database models.
- Generated the first `baseline_schema` migration, capturing the current state of the database.

### 5. DevOps & CI/CD Pipeline
To ensure long-term stability and enforce code quality, we provisioned standard DevOps infrastructure:
- **Backend Dockerfile**: A lightweight Python 3.12 slim image to serve the FastAPI application.
- **Frontend Dockerfile**: A multi-stage Node 20 Alpine image that builds and serves the Next.js application in production mode.
- **GitHub Actions**: Added `.github/workflows/ci.yml` to automatically run Python limits and PyTest, and to verify the Next.js production build on every push and pull request.

## Validation Results

- ✅ **Backend Health**: The `pytest tests/test_routers/test_health.py` test suite passed.
- ✅ **Frontend Build**: `npm run build` completed successfully, proving the TypeScript refactoring of the hooks and `page.tsx` introduced no type violations or missing imports.
- ✅ **Import Resolution**: verified that Python can successfully load the 40+ modularized `TOOL_DEFINITIONS` from the new `ai_tools` package.
