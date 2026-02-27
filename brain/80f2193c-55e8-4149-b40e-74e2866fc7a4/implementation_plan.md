# Make task date and time optional

The user wants to make the date and time fields optional when adding or editing tasks. Currently, the UI might be enforcing these fields or defaulting them in a way that makes them seem required.

## Proposed Changes

### [Component Name] Frontend

#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/tasks/page.tsx)
- **InlineTaskCreator**:
    - Restore initial `dueDate` and `dueTime` to current date and time.
    - Add clear buttons (X) to the date and time input sections.
    - Update `adjustDate` and `adjustTime` to handle cases where the field is currently empty (default to today/now).
    - Ensure `handleSubmit` still handles empty strings as `null`.
    - Reset to defaults after successful submission.
- **TaskDetail**:
    - Add clear buttons to the date and time input sections for easier removal.

### [Component Name] Backend

#### [MODIFY] [test_tasks.py](file:///Users/oli/Desktop/CraftCanvas/backend/tests/test_routers/test_tasks.py)
- Add a test case `test_create_task_without_date` to verify that the backend handles task creation without a due date correctly.

## Verification Plan

### Automated Tests
- Run backend tests: `pytest backend/tests/test_routers/test_tasks.py`
- Run frontend tests (if applicable): `npm test` in `frontend` directory.

### Manual Verification
- Open the application and navigate to the Tasks page.
- Try adding a task using the `InlineTaskCreator` without selecting a date or time.
- Verify the task is created and displayed correctly in the "No date" group.
- Open the detail panel for a task with a date and clear the date and time.
- Save and verify the changes persist.
