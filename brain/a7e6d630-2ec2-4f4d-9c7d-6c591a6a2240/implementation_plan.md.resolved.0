# Implementation Plan - Fix 401 Unauthorized Error

The user is experiencing a 401 Unauthorized error in the planner, which is caused by the backend's in-memory session store being reset every time the server restarts (common in development). The frontend also doesn't handle 401 errors gracefully, staying on the page instead of redirecting to login.

## Proposed Changes

### Backend

#### [MODIFY] [session_store.py](file:///Users/oli/Desktop/CraftCanvas/backend/lib/session_store.py)
- Implement a simple JSON-based persistence for the `sessions` dictionary.
- Sessions will be saved to `data/sessions.json` and loaded on startup.

### Frontend

#### [MODIFY] [api.ts](file:///Users/oli/Desktop/CraftCanvas/frontend/lib/api.ts)
- Dispatch a custom `auth-unauthorized` event when a 401 response is received.

#### [MODIFY] [AuthProvider.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/AuthProvider.tsx)
- Listen for the `auth-unauthorized` event and set the user state to `null`, triggering an automatic redirect to the login page.

## Verification Plan

### Automated Tests
- Run `npm run test` in the frontend (if available) or check for lint errors.
- I will check if there are existing backend tests for authentication.

### Manual Verification
1. Log in to the application.
2. Manually trigger a backend restart (or simulate session loss by clearing `sessions.json`).
3. Navigate to the Planner page or refresh it.
4. Verify that the application redirects to the login page when a 401 is encountered, instead of showing a console error.
5. Verify that sessions persist across normal backend restarts now.
