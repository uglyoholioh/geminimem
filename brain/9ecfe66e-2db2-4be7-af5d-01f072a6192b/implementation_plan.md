# CraftCanvas Comprehensive Test Plan

The goal is to implement a robust testing strategy across both the backend and frontend of CraftCanvas. Currently, the project lacks standard testing frameworks. The strategy covers Unit, Integration, and End-to-End (E2E) testing.

## User Review Required

> [!IMPORTANT]
> Since no testing frameworks are currently installed, this plan proposes standard industry choices: `pytest` for the Python backend, `Vitest` for Next.js unit/component tests, and `Playwright` for E2E tests. Please approve these choices or let me know if you prefer alternatives (e.g., Jest instead of Vitest, Cypress instead of Playwright).

## Proposed Changes

### Backend Comprehensive Testing (Pytest)
The backend testing will focus on decoupling database state, business logic, and API transport.

#### [NEW] backend/tests/test_services/
Unit tests for critical business logic, mocking external API calls:
- `test_canvas_sync.py`: Test Canvas data fetching, diffing, and database syncing logic (with mocked responses).
- `test_brief_generator.py`: Verify prompt assembly and output parsing.
- `test_ics_parser.py` & `test_nusmods.py`: Verify correct parsing of `.ics` files into timetable slots.
- `test_task_parser.py`: Verify natural language extraction parsing into tasks.

#### [NEW] backend/tests/test_routers/
Integration tests for FastAPI endpoints using `TestClient` and an in-memory SQLite DB:
- `test_auth.py`: Login, registration, token generation.
- `test_tasks.py`: CRUD operations, filtering by completion and tags.
- `test_courses.py` & `test_assignments.py`: Fetching courses, verifying assignment urgency sorting and status updates.
- `test_timetable.py`: Week/Day view data retrieval.
- `test_brief.py`: Generation triggering and history retrieval.

#### [NEW] backend/tests/test_models/
- `test_schemas.py`: Validate Pydantic/SQLModel constraints, default values, and relationships.

---

### Frontend Comprehensive Testing (Vitest & React Testing Library)
Frontend testing will focus on complex state interactions and data rendering.

#### [NEW] frontend/hooks/__tests__/
- `useCanvasSync.test.ts`: Test sync status state, polling, and error handling.
- `useZustandStore.test.ts`: (If applicable) Test local client state updates.

#### [NEW] frontend/components/__tests__/
- **Dashboard Views**: `Dashboard.test.tsx` - Verify bento grid layout rendering and data mapping.
- **Task Management**: `TaskItem.test.tsx`, `TaskBoard.test.tsx` - Verify mark-as-done actions, editing, and drag-and-drop structural integrity.
- **Timetable/Calendar**: `Timetable.test.tsx` - Verify rendering of events correctly on the timeline based on passed props.
- **UI Base**: Selected complex shadcn wrappers (e.g., date pickers, select dropdowns).

---

### End-to-End Testing (Playwright)
E2E testing will simulate real user interactions from a fresh state against a localized or mocked backend.

#### [NEW] frontend/e2e/onboarding.spec.ts
- Complete flow: Start from landing -> Register -> Connect Canvas (Mocked) -> Upload ICS -> Arrive at Dashboard.

#### [NEW] frontend/e2e/dashboard_daily_usage.spec.ts
- Complete flow: Read Daily Brief -> Mark a quick task as done -> Change an assignment status -> Generate AI chat query.

#### [NEW] frontend/e2e/task_management.spec.ts
- Complete flow: Navigate to tasks page -> Create new task -> Add tag -> Change due date -> Verify retention on reload.

## Verification Plan

### Automated Tests
Once the structural files are in place, we will run the test runners to verify they execute correctly, even with basic dummy tests.
- Backend: `cd backend && pytest`
- Frontend Unit: `cd frontend && npx vitest run`
- Frontend E2E: `cd frontend && npx playwright test`

### Manual Verification
- We will write 1-2 actual tests for each layer (Backend route, Frontend component, E2E flow) to demonstrate they work realistically.
- User review of exactly which test flows provide the most value immediately.
