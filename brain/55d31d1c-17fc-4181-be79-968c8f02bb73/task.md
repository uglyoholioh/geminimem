# Enhanced AI Chat — Full Context & Actionable Suggestions

## Backend — Expanded System Prompt
- [x] Rewrite `brief_chat` in `routers/brief.py` to query all Canvas data directly
- [x] Inject module PDF excerpts from `ModuleFile.extracted_text`
- [x] Add `:::action` structured output instructions to the system prompt
- [x] Increase Ollama `num_ctx` to 8192 in `ai_service.py`

## Frontend — Dashboard Layout Redesign
- [x] Change grid from 3-col to 2-col (60/40 split) in `app/page.tsx`
- [x] Make Daily Brief chat fill viewport height as hero panel
- [x] Stack Schedule, Tasks, Assignments, Announcements, Stats on right side
- [x] Clean up duplicate `ChatMessage` interface

## Frontend — Action Cards
- [x] Create `lib/parseActions.ts` utility to split AI responses into text + action blocks
- [x] Create `components/chat/ActionCard.tsx` component
- [x] Integrate action card rendering into the chat message display
- [x] Wire "Add Task" button to `POST /api/v1/tasks`

## Verification
- [x] Browser test: dashboard layout validation (2-col, hero chat panel)
