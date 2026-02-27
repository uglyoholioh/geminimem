# Comprehensive Browser Testing Plan

This plan outlines the strategy for headless browser testing of all individual functionalities within the CraftCanvas application, covering edge-to-edge cases.

## Goal
Verify that the frontend correctly interacts with the backend (or mocks), handles all user flows, and gracefully manages edge cases and error states.

## Testing Strategy
We will use **Playwright** for headless browser testing. Tests will be located in `frontend/e2e/verification.spec.ts`.

### 1. Authentication & Onboarding
- [ ] **Login Flow**: Valid/Invalid credentials, password masking.
- [ ] **Registration**: New user registration, email validation, duplicate email check.
- [ ] **Onboarding**: Check if new users are correctly prompted for setup.
- [ ] **Protected Routes**: Ensure unauthorized users are redirected to `/login`.

### 2. Dashboard & Navigation
- [ ] **Sidebar**: Navigation to all pages, collapsible state.
- [ ] **Dashboard Widgets**: Correct display of today's classes, tasks, and assignments.
- [ ] **Greeting**: Personalized greeting based on the time of day and user name.

### 3. Task Management
- [ ] **CRUD Operations**: Create, read (list), update (toggle/edit), delete.
- [ ] **Edge Cases**: Creating tasks with no due date, very long titles, and different priorities.
- [ ] **Filtering**: Verify tasks are correctly filtered/sorted.

### 4. Courses & Assignments
- [ ] **Course List**: Active vs. inactive modules.
- [ ] **Assignment Sync**: Triggering sync and verifying result visibility.
- [ ] **Course Customization**: Updating course colors.

### 5. Timetable & Focus
- [ ] **ICS Import**: Verify schedule parsing (mocked import flow).
- [ ] **Today's View**: display of current and upcoming classes.
- [ ] **Focus Mode**: Verification of the focus timer and UI state.

### 6. AI Brief & Chat
- [ ] **Brief Generation**: Mocking the generation flow and verifying display.
- [ ] **Interactive Chat**: Sending messages and receiving responses.
- [ ] **Action Blocks**: Verifying that `:::action` blocks render correctly and are interactive.

### 7. Meetings (Availability Finder)
- [ ] **Meeting Creation**: Date/time range validation.
- [ ] **Shared Link**: Accessing a shared meeting as a guest.
- [ ] **Availability Submission**: Adding slots as a participant.

## Verification Plan

### Automated Tests
- `npx playwright test e2e/verification.spec.ts`

### Manual Check (via Browser Subagent)
- Visual verification of premium UI elements (animations, glassmorphism, responsive behavior).
