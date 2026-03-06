# Test Plan: Activity Finder

- [x] Navigate to http://localhost:3000/meetings (Redirected to /login)
- [x] Log in or Register (Registered with test_browser_agent@example.com / password123)
- [x] Check if "Activity Finder" heading is visible (Briefly visible before redirect)
- [ ] Create "New Meeting" (Blocked by redirect loop and API 404)

## Summary of Findings & Bugs
1.  **Critical Bug: API 404** - The endpoint `/api/v1/meetings` returns a 404 Not Found. This is likely the root cause of the Activity Finder being broken.
2.  **UI Bug: Pre-filled Email** - The email field on the login and registration pages is pre-filled with the URL `https://connect.craft.do/links/4MBkXoq25FW/api/v1`.
3.  **Auth Bug: 401 Unauthorized** - Even after a "successful" login/registration, the frontend receives 401 for `/api/v1/auth/me` and `/api/v1/settings`. The session/token is not being persisted correctly.
4.  **Setup Lock**: New users are locked in a `/setup` redirect loop that cannot be completed because the "Standard" setup path relies on API calls that are currently failing (401/404).

## Conclusion
The Activity Finder cannot be tested or fixed until the backend routes are corrected and the authentication session persistence is fixed.
