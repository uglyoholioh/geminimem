# CraftCanvas Testing Plan

## Checklist
- [x] Login at http://localhost:3000/login (test@example.com / password123) - SUCCESS
- [x] Verify Dashboard (http://localhost:3000/) - SUCCESS
- [ ] Verify Assignments (http://localhost:3000/assignments) - FAILED (No items shown, 401 error)
- [ ] Verify Grades (http://localhost:3000/grades) - FAILED (Error message shown, 401 error)
- [x] Verify Courses (http://localhost:3000/courses) - SUCCESS (No syntax errors, data loaded)
- [x] Verify Tasks (http://localhost:3000/tasks) - SUCCESS
- [ ] Confirm data loading in Assignments and Grades - FAILED

## Findings
- **Login**: Working correctly.
- **Dashboard, Courses, Tasks**: Working correctly. These pages seem to use relative API paths (e.g., `/api/v1/...`).
- **Assignments & Grades**: These pages are failing because they attempt to fetch data from `http://localhost:8000/api/v1/...` directly. This results in a **401 Unauthorized** error because the session cookies (set on `localhost:3000`) are not sent or accepted on the direct backend port 8000 without proper CORS/credentials configuration.
- **Recommendation**: Update Assignments and Grades frontend code to use relative paths or the standard API utility that handles proxying/base URLs.
