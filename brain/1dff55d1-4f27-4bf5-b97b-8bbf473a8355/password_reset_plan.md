# Implementation Plan: Password Reset Functionality

This plan outlines the steps to add a password reset feature to CraftCanvas. Since there is no integrated email service, the "Forgot Password" request will generate a unique token that the user can use (or an administrator/developer can provide for now) to reset their password.

## User Review Required

> [!NOTE]
> Currently, the system does not have an automated email service. The password reset token will be logged to the backend console for demonstration/development purposes. In a production environment, this token would be sent via email.

## Proposed Changes

### [Component] Backend - Models

#### [NEW] [password_reset_token.py](file:///Users/oli/Desktop/CraftCanvas/backend/models/password_reset_token.py)
Create a new model to store reset tokens:
- `id`: Primary key
- `user_id`: Foreign key to `User`
- `token`: Unique string (UUID or secure token)
- `expires_at`: Expiration timestamp
- `used`: Boolean flag

### [Component] Backend - Routers

#### [MODIFY] [auth.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/auth.py)
Add two new endpoints:
- `POST /forgot-password`: Accept email, verify user exists, generate `PasswordResetToken`, and log it.
- `POST /reset-password`: Accept token and new password, verify token validity, and update user's password.

### [Component] Frontend - Pages & Components

#### [MODIFY] [login/page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/login/page.tsx)
- Add a "Forgot Password?" link below the password field.

#### [NEW] [forgot-password/page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/forgot-password/page.tsx)
- Create a simple form for users to enter their email to request a reset link.

#### [NEW] [reset-password/page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/reset-password/page.tsx)
- Create a form that accepts a token (from URL) and a new password.

## Verification Plan

### Automated Tests
- Create tests in `backend/tests/test_routers/test_auth.py` for:
    - Token generation on request.
    - Password update with valid token.
    - Error on invalid or expired token.

### Manual Verification
- Trigger a "Forgot Password" request and check backend logs for the token.
- Navigate to the reset page with the token and successfully change the password.
- Verify login works with the new password.
