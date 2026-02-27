# Task: Make date and time optional for tasks

- [x] Backend Changes
    - [x] Update `Task` model in `backend/models/task.py` to allow null for date/time fields (Already supports)
    - [x] Update task creation logic in `backend/routers/tasks.py` (Already supports)
    - [x] Update `task_parser` service if needed in `backend/services/task_parser.py` (Already supports)
- [x] Frontend Changes (v1)
    - [x] Update `InlineTaskCreator` to allow empty date/time
    - [x] Update `TaskDetail` to allow empty date/time
- [/] Frontend Changes (v2 - Restore Defaults)
    - [ ] Restore default date/time values in `InlineTaskCreator`
    - [ ] Add clear buttons to date/time inputs in `InlineTaskCreator` and `TaskDetail`
    - [ ] Update `adjustDate` and `adjustTime` logic
- [x] Verification
    - [x] Test creating a task without date/time (clearing defaults)
    - [x] Test creating a task with defaults
    - [x] Verify adjustment buttons work when field is empty
