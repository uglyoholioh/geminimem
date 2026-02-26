# Task: Refine Onboarding Process

## Bug Fix: Repeated Onboarding
- [x] Investigate why onboarding triggers every login <!-- id: 0 -->
    - [x] Identify missing fields in backend login/register responses <!-- id: 1 -->
- [x] Fix backend auth responses <!-- id: 2 -->
    - [x] Update `/auth/login` to include `is_onboarded` and `user_type` <!-- id: 3 -->
    - [x] Update `/auth/register` to include `is_onboarded` and `user_type` <!-- id: 4 -->
- [x] Improve Onboarding UX (Refinement) <!-- id: 5 -->
    - [x] Add loading states and better feedback in `setup/page.tsx` <!-- id: 6 -->
    - [x] Ensure smooth transition to dashboard after completion <!-- id: 7 -->

## Verification
- [x] Verify fix by logging in with an onboarded user <!-- id: 8 -->
- [x] Verify new user registration flow <!-- id: 9 -->
