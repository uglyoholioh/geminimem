# Task: Removing Browser Alerts

- [x] Initial Audit: Identify all instances of `alert`, `confirm`, and `prompt` in the frontend codebase.
- [x] Implement UI Feedback System:
    - [x] Create `uiStore.ts` (Zustand) for global state.
    - [x] Build `Toaster.tsx` for notifications.
    - [x] Build `GlobalDialog.tsx` for confirmations and prompts.
    - [x] Integrate components into `ClientLayout.tsx`.
- [x] Refactor Components:
    - [x] Replace native calls in Timetable (`app/timetable/page.tsx`).
    - [x] Replace native calls in Meetings (`app/meetings/page.tsx`, `app/meetings/[token]/page.tsx`, etc.).
    - [x] Replace native calls in Tasks, Notes, Grades, and Courses.
- [x] Final Verification: Run audit again and verify all interactions work as expected.
