# Intelligent Action Routing & Mutation

We have successfully given the AI "vision" (Stage 3) and "full-stack awareness" (Stage 4). Now we give it "hands" — the ability to **modify data** and **navigate the UI**.

## Proposed Changes

### Backend: Expanding the AI Toolset

---

#### [MODIFY] [ai_tools.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_tools.py)

Add the following mutation and navigation tools:

| Tool | Parameters | Effect |
|------|------------|--------|
| `ui_navigate` | `path: str` | Signals the UI to navigate to a specific page (e.g., `/tasks`, `/assignments`, `/notes/{id}`) |
| `create_task` | `title, priority, due_date, course_code` | Creates a new task in the database. |
| `update_task` | `task_id, **updates` | Updates an existing task (status, priority, etc.) |
| `update_assignment` | `assignment_id, **updates` | Updates an assignment (status, notes). |

---

#### [MODIFY] [ai_service.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_service.py)

- **Tool Interception**: When a "Mutation" or "Navigation" tool is called, the `ai_service` will keep track of it.
- **Action Appending**: After the final response is generated, if a UI-impacting tool was called, the service will append a `:::action` block to the system generated response.
  - *Example*: If `create_task` was called, it appends a block that triggers a frontend refresh.
  - *Example*: If `ui_navigate` was called, it appends a block that renders a "View" or "Go" button.

### Frontend: Reacting to Tool-Triggered Actions

---

#### [MODIFY] [parseActions.ts](file:///Users/oli/Desktop/CraftCanvas/frontend/lib/parseActions.ts)

- Update `ActionData` type to include `navigate` and `refresh` types.

---

#### [MODIFY] [ActionCard.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/chat/ActionCard.tsx)

- Handle `type: 'navigate'`: Uses `next/navigation` `useRouter().push(path)` to move the user.
- Handle `type: 'refresh'`: Simply triggers the `onAdded` callback to refresh the current page data.

## Verification Plan

### Manual Verification
1. **Navigation**: Ask "Take me to my CS101 assignments" → AI should find the course and call `ui_navigate(path='/assignments?module=CS101')`. A "Navigate" card should appear.
2. **Creation**: Ask "Add a high priority task for my midterm tomorrow" → AI calls `create_task`. A "Task Added" card appears with a refresh trigger.
3. **Closing the Loop**: Verify that after the AI says "I've added that", the new task is indeed visible on the dashboard/tasks page without a manual page reload.
