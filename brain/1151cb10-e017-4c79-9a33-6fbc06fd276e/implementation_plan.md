# Daily Brief Chat Integration

Merge AI chat into the Daily Brief widget so users can converse with the AI about their brief (ask follow-ups, refine what's shown, etc.). Remove the standalone `/ai` chat page entirely — the Daily Brief becomes the single AI conversational surface.

## Proposed Changes

### Backend — Brief-Aware Chat Endpoint

#### [MODIFY] [brief.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/brief.py)

Add a new `POST /brief/chat` endpoint that:
1. Loads today's brief data (classes, deadlines, tasks, announcements, prose_text).
2. Injects it as a **system message** so the LLM has full context.
3. Appends the user's conversation history and sends to Ollama via `ai_service.chat()`.
4. Returns the assistant response.

This means every chat message the user sends is automatically grounded in today's brief data — no prompt engineering needed on the frontend.

---

### Frontend — Dashboard Overhaul

#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/page.tsx)

Restructure the dashboard layout significantly:

- **Remove** the "Quick AI" section (right column, lines 392–426) entirely.
- **Transform** the Daily Brief section (left column) into an expandable card that contains:
  1. The generated brief prose (as today), collapsed by default once conversation starts.
  2. A chat thread below the brief — messages displayed inline.
  3. A sticky input bar at the bottom of the brief card.
  4. Suggestion chips (e.g. "What should I focus on?", "Show me only deadlines", "Summarise my week").
- The brief card should **span the full left column** and grow to fill available height.
- Chat calls `POST /api/v1/brief/chat` instead of `/api/v1/ai/chat`.

---

### Frontend — Remove AI Chat Page & Sidebar Link

#### [DELETE] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/ai/page.tsx)

Delete the standalone AI chat page — its functionality is now embedded in the dashboard brief.

#### [MODIFY] [Sidebar.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/layout/Sidebar.tsx)

Remove the `{ href: '/ai', icon: MessageSquare, label: 'AI Chat' }` entry from `NAV_ITEMS`.

---

### Backend — Cleanup

#### [MODIFY] [main.py](file:///Users/oli/Desktop/CraftCanvas/backend/main.py)

Remove the AI router registration:
```diff
-from routers.ai import router as ai_router
-app.include_router(ai_router, prefix="/api/v1/ai", tags=["ai"], dependencies=auth_dep)
```

> [!NOTE]
> The `routers/ai.py` file and `services/ai_service.py` are kept on disk since
> `ai_service` is still used by `brief_generator.py`, `brief.py` (new chat endpoint),
> and `courses/[id]/page.tsx` module brief feature. Only the standalone route mount is removed.

---

## Verification Plan

### Browser Testing
1. Start the backend (`python3 -m uvicorn main:app --reload`) and frontend (`npm run dev`).
2. Navigate to `http://localhost:3000/`.
3. Verify the Daily Brief card renders with the brief prose and a chat input at the bottom.
4. Click "Generate" / "Refresh" to ensure brief generation still works.
5. Type a message like "What should I focus on today?" and press Enter.
6. Verify the AI responds with context-aware content referencing today's schedule/deadlines.
7. Verify the `/ai` route no longer exists (should 404 or redirect).
8. Verify the sidebar no longer shows the "AI Chat" icon.
