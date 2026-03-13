# Detailed Procedural Implementation Plan: Trinity Planner

This document outlines the step-by-step technical implementation of the Trinity Planner. We will proceed procedurally, ensuring each phase is verified before moving to the next.

## Phase 1: Refining 'Capture' (The Inbox)
**Goal:** Create a high-clarity, distraction-free task management experience.

1.  **Backend Enhancements**:
    - Update `Task` model to include an `is_starred` boolean field.
    - Create an API endpoint `PATCH /tasks/{id}/star` to toggle the focus state.
2.  **Frontend: Minimalist Inbox**:
    - Add a "Focus Toggle" (star icon) to task rows in `/tasks`.
    - Create a "Zen Mode" toggle for the task list that hides all non-essential metadata (tags, dates, etc.) to show only the title and course code.
    - Implement a visual indicator for "Today's Limit" (warning if more than 3 items are starred).

## Phase 2: Building 'Strategy' (The Flow)
**Goal:** Implement the drag-and-drop handshake between the Inbox and the Schedule.

1.  **Backend: Unified Planner API**:
    - Implement `GET /planner/items` that returns a mix of starred tasks, assignments, and timetable slots (re-implementing the unified schema approach conceptualized earlier).
2.  **Frontend: The Staging Area**:
    - Modify `/planner` to include a side drawer ("The Vortex") containing only the user's **Starred** tasks.
    - Implement `dnd-kit` or equivalent to allow dragging from the drawer into time slots.
3.  **Frontend: The Zen Timeline**:
    - Refine the `/planner` vertical timeline to be more sparse.
    - Add a "Start Focus" button to each scheduled block.

## Phase 3: Perfecting 'Execution' (The Focus Hub)
**Goal:** Seamless transition into deep work.

1.  **Handshake Logic**:
    - Update `/planner` to pass the `taskId` or `eventId` to the `/focus` page via query params.
2.  **Focus Page Refinement**:
    - Ensure `/focus` can auto-load a task passed via the URL.
    - Add a "Back to Plan" button that returns the user to the specific time-slot they came from in the Flow view.

## Phase 4: Verification & Polish
- End-to-end testing of the Star → Drag → Focus flow.
- Visual polish: Glassmorphism, smooth transitions, and typography refinements.

---

> [!IMPORTANT]
> We will start with **Phase 1: Backend Stars** to establish the data foundation.
