# Refine Onboarding Process and Fix Persistence Bug

The user reports that the onboarding flow triggers on every login. This is caused by the backend `login` and `register` endpoints failing to return the `is_onboarded` flag, causing the frontend to assume onboarding is incomplete. This plan fixes that bug and adds minor refinements to the onboarding UX.

## Proposed Changes

### [Backend] Auth Router
#### [MODIFY] [auth.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/auth.py)
- Update `login` and `register` endpoint return values to include `is_onboarded` and `user_type`.
- This ensures the frontend `AuthProvider` receives the correct state immediately after authentication.

### [Frontend] Setup Page
#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/setup/page.tsx)
- Add a check at the start of `SetupPage` to redirect or show a message if the user is already onboarded (belt-and-suspenders).
- Improve loading states during `finishSetup` to provide clearer feedback.

## Verification Plan

### Automated Tests
- Run backend tests to ensure auth endpoints still work as expected.
```bash
cd backend && pytest tests/test_routers/test_health.py # Basic check
# I will create a new test file test_auth_onboarded.py to specifically verify this fix.
```

### Manual Verification
1. **Login Flow**: Log in as an existing user who has already completed onboarding. Verify that you are redirected to the dashboard (`/`) and NOT the setup page.
2. **Registration Flow**: Register a new user. Verify you are redirected to `/setup`. Complete setup and verify you are redirected to `/`. Log out and log back in, verify you are NOT sent to `/setup` again.
3. **UI Refinement**: Observe the new loading states and transition during onboarding.
