# Testing Academic Life OS

## Plan
1. [ ] Log in at http://localhost:3000/login
    - Email: test@example.com
    - Password: password123
2. [ ] Verify Dashboard data (http://localhost:3000/)
3. [ ] Verify Assignments data (http://localhost:3000/assignments)
4. [ ] Verify Grades data (http://localhost:3000/grades)
5. [ ] Verify Tasks data (http://localhost:3000/tasks)

## Status
- [x] Log in at http://localhost:3000/login (Success as Test User)
- [x] Verify Dashboard data (http://localhost:3000/) - OK (Data loaded: Agenda, Module Hub, User Info)
- [!] Verify Assignments data (http://localhost:3000/assignments) - FAILED (401 Unauthorized because it redirects from port 3000 to 8000)
- [!] Verify Grades data (http://localhost:3000/grades) - FAILED (401 Unauthorized because it redirects from port 3000 to 8000)
- [x] Verify Tasks data (http://localhost:3000/tasks) - OK (Data loaded: "Test task from browser agent")

### Findings
- Tasks API (/api/v1/tasks) works on port 3000.
- Assignments (/api/v1/assignments) and Grades (/api/v1/grades) APIs on port 3000 are returning a 307 redirect to port 8000.
- Since cookies are set for localhost:3000, the requests to localhost:8000 fail with 401.
