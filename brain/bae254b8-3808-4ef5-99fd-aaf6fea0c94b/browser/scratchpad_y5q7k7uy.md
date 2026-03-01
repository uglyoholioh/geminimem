# Task: Verify Login and Dashboard

## Plan
- [x] Open http://localhost:3000/login
- [x] Login with test@example.com / password123
- [x] Verify Dashboard loads
- [x] Check for 401/500 errors in network tab
- [x] Verify functionality (navigate around)

## Findings
- Login successful with test@example.com.
- Dashboard loads correctly with agenda and AI brief.
- Tasks page works: successfully created a task "Test task from browser agent".
- Courses page works: displays modules and summary statistics.
- Timetable page works: shows the class schedule.
- **Issue: Assignments page** shows "No assignments yet" and has a 401 error for `api/v1/assignments` redirecting to port 8000.
- **Issue: Grades page** shows "Failed to load grades. Please try again."
- Other endpoints are functioning normally.
