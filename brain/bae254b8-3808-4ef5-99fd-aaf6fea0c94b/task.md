# Stage 1: State Sync — Implementation Task List

## Backend
- [x] Register `ai_router` in `backend/main.py`
- [x] Create `backend/services/context_assembler.py` (Prompt logic)
- [x] Update `backend/routers/ai.py` to handle `ChatRequest` with `uiContext`
- [x] Implement `_format_ui_context` in `ai_service.py`

## Frontend
- [x] Create `frontend/lib/uiContext.ts` (Types & Helpers)
- [x] Create `frontend/lib/UIContextProvider.tsx`
- [x] Wrap app in `UIContextProvider` (`app/layout.tsx`)
- [x] Integrate `uiContext` into `useChat.ts`
- [x] Integrate `uiContext` reporting into `app/dashboard/page.tsx` (or root)

## Verification
- [x] Verify AI receives visible items in system prompt
- [x] Verify UI update after action execution

# Stage 2: Data Sync Strategy — Implementation Task List

## Backend
- [x] Create `backend/models/sync_log.py`
- [x] Update `SyncLog` in `backend/database.py`
- [x] Implement `GET /api/v1/sync/freshness` endpoint
- [x] Update `canvas_sync.py` to log results to `SyncLog`

## Frontend
- [x] Create `frontend/components/chat/SyncIndicator.tsx`
- [x] Add `SyncIndicator` to `ConversationPanel`
- [x] Implement periodic freshness check

## Verification
- [x] Verify freshness dot turns yellow/red after simulated time
- [x] Verify "Sync Now" trigger works from chat header

# Stage 3: Component State Awareness — Implementation Task List

## Backend
- [x] Implement `schema_registry.py` for component serialization
- [x] Implement compact serializers for Tasks, Assignments, Timetable

## Frontend
- [x] Implement `frontend/lib/componentSchemas.ts`
- [x] Update `DashboardPage` to use deep state reporting
- [x] Implement state reporting for `TasksPage` and `AssignmentsPage`

## Verification
- [/] Verify AI system prompt contains detailed item state (priority, status, etc.)
- [/] Verify contextual resolution of "the high priority task" works

# Stage 4: AI Full Data Access via Gemini Function Calling

## Backend
- [x] Create `backend/services/ai_tools.py` with 9 tool functions
- [x] Update `ai_service.py` to pass tool declarations to Gemini
- [x] Implement tool call loop (execute → feed back → generate response)
- [x] Update `context_assembler.py` to inform AI about available tools

## Verification
- [x] Test cross-domain queries (assignments for a specific course)
- [x] Test queries beyond the current page (future timetable, all notes)

# Stage 4: Intelligent Action Routing

## Backend
- [x] Add `ui_navigate` tool to `ai_tools.py`
- [x] Add mutation tools (`create_task`, `update_task`, etc.) to `ai_tools.py`
- [x] Implement automatic `:::action` block appending in `ai_service.py` for UI actions

## Frontend
- [x] Update `ActionData` type in `parseActions.ts` to include `navigate`
- [x] Implement automatic/manual navigation in `ActionCard.tsx` (or a new `ActionRouter`)

## Verification
- [x] Verify AI can "take me to the assignments page"
# Stabilization & Login Fixes
- [x] Fix `ClientLayout.tsx` build and render crashes
- [x] Restore missing UI component imports (Toaster, Tooltip)
- [x] Start backend process and verify port 8000
- [x] Fix `SyncIndicator.tsx` missing UI components
- [x] Fix `CoursesPage.tsx` syntax error and trailing slashes
- [x] Resolve 401 Unauthorized redirects on Assignments and Grades
- [x] Verify full app accessibility headless
- [x] Final end-to-end verification of all core modules
