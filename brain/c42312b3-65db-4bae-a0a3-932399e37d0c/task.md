# Task: Debug API and Schema Errors

- [x] Spotify API Fix
    - [x] Resolved `NameError: name 'UTC' is not defined`
    - [x] Unified timezone handling to use `now_utc()`
- [x] Google Calendar API Fix
    - [x] Resolved same `UTC` NameError in `google_calendar.py`
- [x] Meetings API Fix
    - [x] Unified timezone handling using `now_sg()`
- [x] Database Schema Verification
    - [x] Confirmed `backend/data/db.sqlite` has correct columns (`canvas_web_url`, `is_processed`)
    - [x] Validated schema against application logs
- [x] Companion API (404 Error) Fix
    - [x] Resolved trailing slash inconsistency in `app/page.tsx`
- [x] Frontend Quality Assurance
    - [x] Fixed major structural and scoping bugs in `CompanionSprite.tsx`
    - [x] Resolved lint errors in `CompanionSprite.tsx`
- [x] Final Verification
    - [x] Audited all routers for similar NameErrors or timezone issues
