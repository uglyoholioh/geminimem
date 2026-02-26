# Enhanced AI Chat: Full Canvas Context, Module PDF Knowledge & Actionable Suggestions

Upgrade the Daily Brief chat from a brief-only assistant into a full-context academic copilot. The AI gets access to **all Canvas data** and **uploaded module PDFs**, can **suggest actionable items** (add task, schedule event) that the user can execute with one click from inside the chat, and the **dashboard layout** is reorganised to make the Daily Brief the hero component.

## Proposed Changes

### Backend — Expanded System Prompt

#### [MODIFY] [brief.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/brief.py)

Rewrite the `brief_chat` endpoint to build a much richer system prompt:

1. **Full Canvas data** — query `Course`, `Assignment`, `Announcement`, `GradeComponent`/`GradeEntry`, and `Task` models directly (instead of relying only on the serialised brief snapshot).
2. **Module PDF context** — for each active course, pull `ModuleFile.extracted_text` (capped at ~2k chars per file, ~8k total) and inject as a `## Module Materials` section.
3. **Structured actions instruction** — instruct the LLM to embed `ACTION` blocks when suggesting tasks or schedule items. Format:

```
:::action
{"type":"add_task","title":"Review CS2103T lecture notes","priority":"high","due_date":"2026-02-27","course_code":"CS2103T"}
:::
```

The backend passes the raw response through; the frontend parses these blocks.

---

### Frontend — Dashboard Layout Redesign

#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/page.tsx)

1. **Layout**: Switch from `grid-cols-3` to a **2-column layout** — Daily Brief chat takes the left 60% (`col-span-3` in a `grid-cols-5`), and a stacked right sidebar (`col-span-2`) holds Schedule, Tasks, Assignments, Announcements, and Stats.
2. **Daily Brief height**: Make it `calc(100vh - 160px)` so it fills the viewport like a proper chat panel.
3. **Action cards in chat**: Parse `:::action ... :::` blocks from AI responses. Render each as a styled card with an **"Add Task"** / **"Schedule"** button. Clicking the button calls the existing `POST /api/v1/tasks` endpoint and shows a success toast inline.

---

### Frontend — Action Card Component

#### [NEW] [ActionCard.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/chat/ActionCard.tsx)

A small component that:
- Accepts an action JSON object (`type`, `title`, `priority`, `due_date`, `course_code`)
- Renders it as a rounded card with the task info and a CTA button
- On click, calls `api.post('/tasks', ...)` and transitions to a "✓ Added" state
- Supports `add_task` type initially (extensible to `schedule_event` later)

---

### Frontend — Message Parser Utility

#### [NEW] [parseActions.ts](file:///Users/oli/Desktop/CraftCanvas/frontend/lib/parseActions.ts)

Utility to split an AI response string into segments of `text` and `action` blocks so the chat renderer can alternate between prose and interactive action cards.

---

## User Review Required

> [!IMPORTANT]
> The expanded system prompt will include **all courses, all open assignments, all open tasks, grades, announcements, and module PDF excerpts**. This gives the AI comprehensive context but will consume more tokens per request. With `num_ctx: 4096` on Ollama this may be tight — I'll increase it to `8192` in the AI service options.

> [!WARNING]
> Actionable suggestions are **AI-proposed, user-confirmed**. The AI cannot directly mutate data — the user must click the "Add" button on each suggestion card. This is a deliberate safety choice.

## Verification Plan

### Automated Tests
- `POST /api/v1/brief/chat` with a message like "suggest some tasks for this week" — verify response contains `:::action` blocks
- Frontend: send a message, verify action cards render, click "Add Task", verify task appears in task list

### Manual Verification
- Open dashboard, generate a brief, ask "What should I focus on based on my module materials?"
- Verify the AI references uploaded PDF content
- Ask "Suggest tasks for my upcoming deadlines" — verify clickable action cards appear
- Click an action card — verify task is created and card transitions to "Added"
