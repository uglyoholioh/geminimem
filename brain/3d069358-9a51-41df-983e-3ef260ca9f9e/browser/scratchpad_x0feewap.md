# Debugging Login

- [x] Navigate to http://localhost:3000/login
- [x] Log in with test@example.com / password123
- [x] Check for 500 error or success
- [x] Report findings

**Findings:**
- Navigated to http://localhost:3000/login
- Logged in with test@example.com / password123
- Observed "An internal server error occurred."
- Network logs confirm a 500 Internal Server Error for `POST /api/v1/auth/login`.
