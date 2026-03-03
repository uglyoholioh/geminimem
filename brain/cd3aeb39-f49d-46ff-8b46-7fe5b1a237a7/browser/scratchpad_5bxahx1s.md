# Task: Test Chat Functionality at localhost:3000

## Checklist
- [x] Navigate to http://localhost:3000
- [x] Focus the chat input
- [x] Type "Add a new task: Finish probability quiz by Friday" and press Enter
- [x] Observe for "unexpected error"
- [x] Capture screenshot of failure
- [x] Check logs for failure details
- [x] Report outcome

## Notes
- Logged in as Oli.
- Test query: "Add a new task: Finish probability quiz by Friday" at 01:40 PM.
- Result: "I'm sorry, I was unable to add the task at this time due to an unexpected error. Please try again later."
- Console Logs: Multiple `401 (Unauthorized)` errors for `/api/v1/auth/me`, `/api/v1/settings`, `/api/v1/timetable`, etc.
- Detail: API response `{"detail":"Not authenticated"}`.
