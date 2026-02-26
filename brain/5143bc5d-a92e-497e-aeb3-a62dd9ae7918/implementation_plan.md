# Implementation Plan - Fix Timetable Import Multi-User Isolation

Fix the `sqlite3.IntegrityError` when importing ICS files and ensure all timetable operations are isolated per user.

## Proposed Changes

### Backend

#### [MODIFY] [timetable.py](file:///Users/oli/Desktop/LMSManager/backend/app/routes/timetable.py)

- Add `current_user: User = Depends(get_current_active_user)` to the following endpoints:
    - `import_ics_timetable`
    - `import_nusmods_timetable`
    - `import_image_timetable`
    - `import_timetable_data`
    - `clear_timetable`
- Update the `.delete()` calls in these endpoints to filter by `ClassSchedule.user_id == current_user.id`.
- Update the `ClassSchedule` instantiation in these endpoints to include `user_id=current_user.id`.

## Verification Plan

### Automated Tests
- I will create a temporary test script to verify that:
    1.  Importing an ICS file (simulated with content) correctly associates the records with the current user.
    2.  Importing for User A does not overwrite records for User B.
    3.  Clearing the timetable only affects the current user's records.

### Manual Verification
1.  Log in to the application.
2.  Navigate to the settings or timetable page where the ICS import is located.
3.  Upload an ICS file.
4.  Verify that the import succeeds without the `IntegrityError`.
5.  Verify that the imported classes appear in the timetable.
