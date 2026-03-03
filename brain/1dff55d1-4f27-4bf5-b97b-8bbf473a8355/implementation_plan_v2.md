# Fix Internal Server Error for Existing Emails during Registration

The backend currently throws a `500 Internal Server Error` when a user attempts to register with an email address that is already in use. This happens because the `sqlite3.IntegrityError` for the unique constraint on the email field is not caught and handled.

## Proposed Changes

### Backend

#### [MODIFY] [auth.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/auth.py)
- Update the `register` endpoint to check if a user with the given email already exists before proceeding with creation.
- If the user exists, return a `400 Bad Request` or `422 Unprocessable Entity` with a clear message: "Email already registered."

## Verification Plan

### Automated Tests
- Create a new test case in `backend/tests/test_routers/test_auth.py` to verify that registering with an existing email returns a proper error instead of crashing.
- Command: `pytest backend/tests/test_routers/test_auth.py`

### Manual Verification
- Attempt to register with the email `oliverkoh96@gmail.com` (which already exists) and verify that the API returns a structured error message instead of a `500 Internal Server Error`.
