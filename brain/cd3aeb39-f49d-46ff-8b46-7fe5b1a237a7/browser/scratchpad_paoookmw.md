# Task: Add a new task via AI chat

## Plan
- [ ] Navigate to http://localhost:3000 (Blocked by 500 errors/Backend down)
- [ ] Focus chat input and type "Add a new task: Finish probability quiz by Friday"
- [ ] Press Enter and wait for AI response
- [ ] Capture screenshot of confirmation
- [ ] Verify task appears in the task list
- [ ] Report final result

## Progress
- Navigated to http://localhost:3000, redirected to /login.
- Login, Register, and Health endpoints all return 500 Internal Server Error.
- Direct check of backend on http://localhost:8000/docs resulted in `net::ERR_CONNECTION_REFUSED`.
- Conclusion: The backend server failed to start correctly or crashed after the recent edits/restart.
- Task cannot be completed via UI due to backend unavailability.
