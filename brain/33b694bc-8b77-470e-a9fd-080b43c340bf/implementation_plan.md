# Architecture & Code Quality Improvements

Comprehensive plan to reduce duplication, improve maintainability, and enforce consistency across the CraftCanvas codebase.

---

## P0 — Backend: Centralise Timezone Handling

`SG_TZ = pytz.timezone("Asia/Singapore")` and `_now_sg()` are **copy-pasted into 12+ files** (models, services, routers). Any change to timezone policy requires touching every file.

### [NEW] `backend/lib/timezone.py`
```python
import pytz
from datetime import datetime

SG_TZ = pytz.timezone("Asia/Singapore")

def now_sg() -> datetime:
    return datetime.now(SG_TZ)
```

### [MODIFY] All model files that define `SG_TZ` / `_now_sg`
Replace local definitions with:
```python
from lib.timezone import SG_TZ, now_sg
```

**Files affected** (each currently has its own copy):
- `models/announcement.py`, `models/assignment.py`, `models/course.py`, `models/task.py`, `models/user.py`, `models/meeting.py`, `models/note.py`, `models/event.py`, `models/craft_block.py`
- `services/brief_generator.py`, `services/craft_sync.py`, `services/context_providers.py`, `services/notification_service.py`
- `routers/timetable.py`, `routers/assignments.py`, `routers/courses.py`, `routers/notes.py`, `routers/tasks.py`

---

## P0 — Backend: Extract Session Store from Router

`dependencies.py` does `from routers.auth import _sessions` — a circular dependency risk. The session store should live in its own module.

### [NEW] `backend/lib/session_store.py`
Move `_sessions: dict[str, int]` here. Both `routers/auth.py` and `dependencies.py` import from this module.

### [MODIFY] `backend/routers/auth.py`
Import `_sessions` from `lib/session_store` instead of defining it.

### [MODIFY] `backend/dependencies.py`
Import from `lib/session_store` instead of `routers.auth`.

---

## P1 — Backend: Delete Dead Middleware

### [DELETE] `backend/middleware.py`
`APIKeyMiddleware` is **never mounted** in `main.py` — auth is handled entirely via `dependencies.py`. This file is dead code and creates confusion about the auth model.

---

## P1 — Backend: Break Up Giant Timetable Router

`routers/timetable.py` is **34KB / 900+ lines** with mixed concerns (CRUD, NUSMods import, ICS parsing, attendance, exam scheduling). Split into focused modules:

### [MODIFY] `backend/routers/timetable.py`
Keep only core CRUD: list slots, create/update/delete slot, get today/week schedule.

### [NEW] `backend/routers/timetable_import.py`
Move NUSMods import and ICS import endpoints here.

### [NEW] `backend/routers/timetable_attendance.py`
Move attendance tracking endpoints here.

---

## P1 — Frontend: Extract Chat Hook

`handleChatSend`, chat state (`chatMessages`, `chatInput`, `chatLoading`), and the `DailyBriefChat` drawer are **duplicated across 6 pages**: dashboard, brief, announcements, assignments, tasks, and course detail.

### [NEW] `frontend/lib/useChat.ts`
Custom hook encapsulating:
- `messages`, `input`, `setInput`, `loading` state
- `sendMessage(text?)` — streams via `api.stream`
- `handleActionAdded()` callback integration

### [MODIFY] Pages using chat
Each page replaces ~40 lines of boilerplate with `const chat = useChat({ onAction })`.

**Pages affected**: `page.tsx` (dashboard), `brief/page.tsx`, `announcements/page.tsx`, `assignments/page.tsx`, `tasks/page.tsx`, `courses/[id]/page.tsx`

---

## P1 — Frontend: Break Up `tasks/page.tsx` (1706 lines)

This single file contains the default export, `TasksContent`, `InlineTaskCreator`, `TaskRow`, `TimelineView`, `TaskDetail`, plus all helpers — making it very hard to navigate and test.

### [NEW] `frontend/components/tasks/TaskRow.tsx`
Move `TaskRow` component (~120 lines).

### [NEW] `frontend/components/tasks/TaskDetail.tsx`
Move `TaskDetail` panel (~350 lines).

### [NEW] `frontend/components/tasks/InlineTaskCreator.tsx`
Move `InlineTaskCreator` (~190 lines).

### [NEW] `frontend/components/tasks/TimelineView.tsx`
Move `TimelineView` (~70 lines).

### [MODIFY] `frontend/app/tasks/page.tsx`
Import the extracted components. Should drop from ~1700 to ~500 lines.

---

## P2 — Frontend: Eliminate `useState<any>`

`useState<any>` appears in **11 files**, defeating TypeScript's safety. Replace with proper types from `lib/types.ts` or inline interfaces.

**Files affected**: `CommandPalette.tsx`, `KanbanBoard.tsx`, `meetings/[token]/page.tsx`, `announcements/page.tsx`, `grades/page.tsx`, `assignments/page.tsx`, `tasks/page.tsx`

---

## P2 — Backend: Add Pydantic Response Schemas

Routers return raw `dict` or model instances without explicit response schemas. Adding `response_model` to endpoints would:
- Auto-generate OpenAPI docs
- Prevent accidental data leaks (e.g. API keys)
- Enforce consistent response shapes

### Create `backend/schemas/` directory
With files like `task_schemas.py`, `assignment_schemas.py` containing Pydantic `BaseModel` response classes.

### Modify routers
Add `response_model=` parameter to each endpoint decorator.

---

## P2 — Frontend: Add Error Boundaries & Loading Abstraction

No pages handle fetch errors gracefully — they silently `catch` and `console.error`. A crash in any component takes down the entire page.

### [NEW] `frontend/components/ErrorBoundary.tsx`
React error boundary with a styled fallback UI and retry button.

### [NEW] `frontend/lib/useFetch.ts`
Hook wrapping `api.get` with `{ data, loading, error, refetch }` pattern, replacing manual `useState`+`useEffect`+`try/catch` in every page.

---

## Summary Table

| ID | Area | Effort | Impact | Description |
|----|------|--------|--------|-------------|
| P0-1 | Backend | Small | High | Centralise `SG_TZ` / `now_sg()` into `lib/timezone.py` |
| P0-2 | Backend | Small | High | Extract session store out of `routers/auth` |
| P1-1 | Backend | Tiny | Medium | Delete dead `middleware.py` |
| P1-2 | Backend | Medium | High | Split timetable router into 3 modules |
| P1-3 | Frontend | Medium | High | Extract `useChat` hook from 6 pages |
| P1-4 | Frontend | Medium | High | Break up `tasks/page.tsx` into components |
| P2-1 | Frontend | Small | Medium | Replace `useState<any>` with typed state |
| P2-2 | Backend | Large | Medium | Add Pydantic response schemas |
| P2-3 | Frontend | Medium | Medium | Add error boundaries & `useFetch` hook |

## Verification Plan

### Automated Tests
- Run `pytest` in backend to confirm no regressions from timezone/session refactors
- Run `npm run build` in frontend to catch TypeScript errors from component extraction
- Run existing Playwright E2E tests

### Manual
- Spot-check dashboard, tasks, assignments, and timetable pages in browser
