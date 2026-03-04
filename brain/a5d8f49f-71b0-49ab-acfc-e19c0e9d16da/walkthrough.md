# Walkthrough - Login Issue Resolution

The login issue was caused by a backend crash on startup. This crash occurred because several files were still trying to import and use the "Grades" models, which had been removed in a previous task.

## Changes Made

### Backend Stabilisation
- Cleaned up `backend/routers/courses.py`: removed `Grade` imports and logic from the course detail endpoint.
- Cleaned up `backend/routers/brief.py`: removed `Grade` data from the AI assistant context.
- Updated `backend/database.py`: removed `Grade` model registration.
- Updated `backend/tests/conftest.py`: removed `Grade` model registration from the test database setup.

### Verification Results
1. **Server Health**: Confirmed the backend now starts successfully on port 8000 without `ModuleNotFoundError`.
2. **Endpoint check**: 
   - `/api/v1/auth/status` returns `{"has_users": true}`, confirming the database and auth router are operational.
   - `/api/v1/auth/login` correctly identifies an invalid password with a `401` status, confirming the authentication flow is working.

## Verification Clips

The following log shows the successful server startup and healthy endpoint responses:

```text
INFO:     Uvicorn running on http://0.0.0.0:8000 (Press CTRL+C to quit)
INFO:     Started server process [12749]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
Checking auth status...
Status: 200, Body: {'has_users': True}
Attempting login for oliverkoh96@gmail.com...
Login Response: 401, Body: {'detail': 'Invalid email or password.'}
```
