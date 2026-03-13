# Task: Debug Login and Cookie Issues

## Checklist
- [x] Go to http://localhost:3000/login
- [x] Log in with test@example.com / password123
- [x] Check if "session" cookie is present (Application tab / document.cookie)
- [x] Check network request to /api/v1/auth/me for "session" cookie header
- [x] Report status of /api/v1/auth/me

## Findings
- Login POST to `/api/v1/auth/login` (reqid 48) returned 200.
- Response set a cookie named `session_token`, NOT `session`.
- The cookie is `HttpOnly`, so it doesn't show up in `document.cookie`.
- Request to `http://localhost:3000/api/v1/auth/me` (reqid 58) returned 200 and included the `session_token` cookie in the request headers.
- Requests to `http://localhost:8000/api/v1/companion/` (reqid 55) failed with 401 because the `session_token` cookie was NOT sent (cross-origin port mismatch).
- The app appears to redirect back to `/login` when these secondary requests fail.
- Status of `/api/v1/auth/me` is 200 (OK).
