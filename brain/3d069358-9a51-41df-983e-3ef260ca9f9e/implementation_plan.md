# Fix Session Persistence and Login Rerouting

The user is experiencing issues where they are rerouted to the login page after logging in. This is caused by the backend's in-memory session store (`session_store.py`) losing state during server reloads or due to unsynchronized in-memory state in multi-worker environments. Although it attempts to persist to `sessions.json`, the loading/saving logic is fragile and prone to race conditions or stale data.

## Proposed Changes

### Backend

#### [MODIFY] [session_store.py](file:///Users/oli/Desktop/CraftCanvas/backend/lib/session_store.py)
- Improve the `load_sessions` and `save_sessions` functions to be more robust.
- Ensure that `load_sessions` is called before every session check if possible, or at least ensure the in-memory map is synchronized with the file.
- Add logging to track when sessions are loaded and saved.

#### [MODIFY] [auth.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/auth.py)
- Explicitly call `load_sessions()` before accessing the `sessions` dictionary in the `logout` and other relevant endpoints to ensure the latest data is used.

### Verification Plan

#### Automated Tests
- Run `backend/prepare_test_user.py` to ensure local DB has a user.
- Create a script `backend/test_session_persistence.py` that:
    1. Calls the login API.
    2. Verifies `sessions.json` is updated.
    3. Manually re-initializes the session store and checks if the session is still valid.

#### Manual Verification
- Log in through the browser.
- Verify that the session persists across page refreshes.
- Manually restart the backend server and verify that the session is still valid (not redirected to /login).
