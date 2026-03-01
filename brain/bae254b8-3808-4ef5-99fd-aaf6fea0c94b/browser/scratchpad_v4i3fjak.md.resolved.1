# Browser Task Checklist
- [x] Open http://localhost:3000/login
- [x] Login with test@example.com / password123
- [x] Visit /assignments: "No assignments yet" shown, API return 401.
- [x] Visit /grades: "Failed to load grades" shown, API return 401.
- [x] Provide summary

## Findings
- Login was successful: Dashboard shows correctly (Timetable, Tasks, Module Hub).
- Assignments and Grades pages are broken due to a trailing slash mismatch in API calls.
  - Frontend calls `/api/v1/assignments` (no slash).
  - Backend redirects to `http://localhost:8000/api/v1/assignments/` (absolute URL with port 8000).
  - Browser treats the absolute redirect to port 8000 as cross-origin and fails with 401 Unauthorized.
- **Root Cause Confirmed**: Manually visiting `http://localhost:3000/api/v1/assignments/` (with slash on port 3000) works perfectly and returns backend data.
- The same applies to `/api/v1/grades/`.
