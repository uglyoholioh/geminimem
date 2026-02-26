# Add Attendance Toggle to Timetable

- [x] Inspect database schema and add `is_attended` to `TimetableSlot`.
- [x] Add `is_attended` column to SQLite database or recreate table if needed.
- [x] Add `PATCH /timetable/slots/{slot_id}` endpoint in backend.
- [x] Update frontend `TimetableSlot` type.
- [x] Add UI toggle to `TimetablePage` in frontend.
- [x] Add UI toggle to Dashboard Today schedule in frontend.
- [x] Test the toggle functionality manually or via e2e tests.

## Review
All functionality works as expected.
