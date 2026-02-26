# Dashboard Today's Agenda Implementation Plan

## Problem Description
The dashboard currently has a "Today's Schedule" section that only displays classes. To provide a better daily overview, this should be converted into a "Today's Agenda" that interleaves classes, tasks due today, and assignments due today into a single chronologically sorted timeline.

## Proposed Changes

### 1. Unified Agenda Data Structure
Create a unified list of agenda items within the `DashboardPage` component's render cycle (or via an effect/memo).
- Filter `assignments` where `due_at` is today.
- Filter `tasks` where `due_date` is today.
- Map `todaySlots` (classes), today's `assignments`, and today's `tasks` into a common interface:
  ```typescript
  interface AgendaItem {
    id: string; // unique identifier
    type: 'class' | 'task' | 'assignment';
    timeString: string; // HH:mm for sorting
    sortValue: number; // minutes from midnight
    data: any; // the underlying TimetableSlot | Task | Assignment
  }
  ```
- Sort the combined array chronologically by the `timeString` equivalent.

### 2. Update UI for "Today's Agenda"
- Rename "Today's Schedule" header to "Today's Agenda" (and change icon if appropriate, e.g. from `Calendar` to `ListTodo` or a combined view icon).
- Map through the sorted `agendaItems` array.
- For `type === 'class'`, use the existing timeline UI (the class card with the active pulse indicator).
- For `type === 'task'`, render a timeline item using the task UI. Include the interactive toggle for completion status and delete button, but style it to fit within the timeline layout (e.g. standardizing the left-side dot/icon and the vertical connecting line).
- For `type === 'assignment'`, render a timeline item using the assignment UI. Include the toggle and delete button, adapted for the timeline layout.

### 3. File modifications
#### [MODIFY] `frontend/app/page.tsx`
- Add utility function to check if a date string is "today" in local time.
- Add utility function to extract `HH:mm` from ISO datetime strings to sort tasks and assignments chronologically against classes.
- Update the block currently rendering `Today's Schedule` to implement the above data mapping, sorting, and polymorphic rendering based on `item.type`.

## Verification Plan
### Automated Tests
- N/A for this visual UI change without explicit E2E tests for the agenda.

### Manual Verification
1. Open the dashboard.
2. Verify that the "Today's Agenda" section appears instead of "Today's Schedule".
3. Verify that if there are tasks or assignments due today, they appear correctly interleaved with the classes in chronological order.
4. Verify that the interactive elements (checkboxes, attendance toggle) work directly from within the Agenda.
