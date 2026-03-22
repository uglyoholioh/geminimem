# Technical Implementation Plan: CraftCanvas Remediation

This plan details the technical steps to resolve the issues identified in the architectural evaluation.

## User Review Required
Please review the order and scope of these tasks. Some tasks (like Alembic migrations) introduce new dependencies (`alembic`) that will require recreating the environment or updating `requirements.txt`.

## Proposed Changes

---

### Section 1: Fix Priority Bugs

#### [MODIFY] main.py (file:///Users/oli/Desktop/CraftCanvas/backend/main.py)
- Move the `@app.middleware("http")` block out of the `global_exception_handler` function so it registers properly with FastAPI.

#### [MODIFY] scheduler.py (file:///Users/oli/Desktop/CraftCanvas/backend/jobs/scheduler.py)
- In `start_scheduler()`, add `scheduler.add_job(calendar_sync_job, IntervalTrigger(minutes=60), id="calendar_sync", replace_existing=True)` to ensure the calendar job actually runs.

#### [MODIFY] ai_tools.py (file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_tools.py)
- In `get_courses()`, replace `results = session.exec(statement).all()` with `results = session.exec(select(Course).where(Course.user_id == user_id, Course.is_active == True)).all()` (or equivalent logic), since `statement` is undefined.

---

### Section 2: Backend Refactoring (`ai_tools.py`)

The 1,926-line `ai_tools.py` will be split into a package: `backend/services/ai_tools/`.

#### [NEW] ai_tools/__init__.py (file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_tools/__init__.py)
- Aggregate all tool functions into a single `TOOL_DEFINITIONS` list to expose to `ai_service.py` without breaking existing imports.

#### [NEW] ai_tools/courses.py (file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_tools/courses.py)
- Move `get_courses`, `get_module_details`, `get_module_files`, `list_course_folders`, `search_announcements`.

#### [NEW] ai_tools/tasks.py (file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_tools/tasks.py)
- Move `search_tasks`, `get_task_stats`, `create_task`, `update_task`, `delete_task`, `search_assignments`, `update_assignment`.

#### [NEW] ai_tools/materials.py (file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_tools/materials.py)
- Move `search_module_materials` (RAG), `get_file_metadata`, `verify_file_exists`, `search_craft_docs`.

#### [NEW] ai_tools/misc.py (file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_tools/misc.py)
- Move `get_timetable`, `search_notes`, `create_note`, `update_note`, `ui_navigate`.

#### [DELETE] ai_tools.py (file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_tools.py)
- Remove the monolith file.
- Update imports in `backend/services/ai_service.py` to point to `from services.ai_tools import TOOL_DEFINITIONS`.

---

### Section 3: Frontend Refactoring (`page.tsx`)

The 730-line dashboard will be refactored by moving logical chunks of state and `useEffect`s into custom hooks.

#### [NEW] useDashboardData.ts (file:///Users/oli/Desktop/CraftCanvas/frontend/hooks/useDashboardData.ts)
- Will encapsulate state for `tasks`, `assignments`, `announcements`, `allCourses`, and `brief`.
- Will export a `refreshData` function and loading states.

#### [NEW] useDashboardChat.ts (file:///Users/oli/Desktop/CraftCanvas/frontend/hooks/useDashboardChat.ts)
- Will track `chatMessages`, `chatInput`, `chatLoading`.
- Will expose `handleChatSend` and `handleClearChat` using the API streaming utility.

#### [NEW] useDashboardTimetable.ts (file:///Users/oli/Desktop/CraftCanvas/frontend/hooks/useDashboardTimetable.ts)
- Will encapsulate `todaySlots` state, `activeLookahead`, and the logic that iterates over following days when today has no classes.

#### [MODIFY] page.tsx (file:///Users/oli/Desktop/CraftCanvas/frontend/app/page.tsx)
- Import the new hooks to sharply reduce component size.
- Retain only local UI state (like `isEditing`, `isManageModules`, `isTriageOpen`) and render logic.

---

### Section 4: Database Migrations (Alembic)

#### [NEW] alembic.ini & alembic/ (file:///Users/oli/Desktop/CraftCanvas/backend/alembic.ini)
- Run `alembic init alembic`.
- Modify `alembic/env.py` to import SQLModel and all model definitions so it can auto-generate migrations.
- Create an initial migration that matches current production SQLite schema.

---

### Section 5: DevOps & CI/CD Setup

#### [NEW] Backend Dockerfile (file:///Users/oli/Desktop/CraftCanvas/backend/Dockerfile)
- Python 3.11 slim image.
- Installs requirements, runs Uvicorn.

#### [NEW] Frontend Dockerfile (file:///Users/oli/Desktop/CraftCanvas/frontend/Dockerfile)
- Node 20 alpine image.
- Runs `npm ci` and `npm run build`.

#### [NEW] ci.yml (file:///Users/oli/Desktop/CraftCanvas/.github/workflows/ci.yml)
- Basic GitHub Actions workflow to check Python types/lint, run tests, and build both Docker images on PR to ensure sanity.

#### [MODIFY] .gitignore (file:///Users/oli/Desktop/CraftCanvas/.gitignore)
- Add `*.bak` and relevant ignore paths.
