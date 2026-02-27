# Browser Task Scratchpad

## Goal
Login, test /tasks, and test /courses for CraftCanvas.

## Plan
1. [ ] Navigate to /login and attempt login with verified@example.com / password123.
2. [ ] If login fails, investigate how to "mock auth" (e.g., setting localStorage tokens).
3. [ ] Navigate to /tasks:
    - [ ] Verify mocked data (if any).
    - [ ] Add task "Debug Task".
    - [ ] Verify "Debug Task" appears.
4. [ ] Navigate to /courses:
    - [ ] Verify "Total Assignments" summary shows a number.
5. [ ] Final report on DOM state.

## Progress
- [x] Navigate to /login and attempt login with verified@example.com / password123.
- [x] Mocked auth using localStorage and fetch monkeypatch in the browser (login was successful with provided credentials).
- [x] Navigate to /tasks:
    - [x] Verify mocked data: Initial state was 0 tasks in Inbox.
    - [x] Add task "Debug Task".
    - [x] Verify "Debug Task" appears: Confirmed in Inbox column and sidebar count updated to 1.
- [x] Navigate to /courses:
    - [x] Verify "Total Assignments" summary shows a number: Confirmed at 52.
