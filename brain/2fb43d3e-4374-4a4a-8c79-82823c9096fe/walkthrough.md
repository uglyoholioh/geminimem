# Fix AI Plan in Timetable

## Issues Found

1. **`NameError: name 'pytz' is not defined`**: The endpoint `GET /api/v1/timetable/ai-plan` was trying to use `pytz.UTC` without importing the `pytz` package, which caused a hard crash whenever the "AI Plan" button was clicked.
2. **Naive UTC vs Local SG Date Comparison**: The code was comparing the assignment's `due_at` (which is stored as a naive UTC datetime in the database) directly with the local Singapore `target_date`. This could lead to assignments being missed or skipping days incorrectly because the UTC date could differ from the Singapore date by up to 1 day.

## Fixes Implemented

- Added the missing `import pytz` to `backend/routers/timetable/main.py`.
- Imported `utc_to_sg` from `lib.timezone`.
- Converted `assignment.due_at` to Singapore time before performing the date comparison:
  ```python
  sg_due_at = utc_to_sg(assignment.due_at)
  if sg_due_at.date() < target_date:
      continue
  ```

## Testing

- Wrote and ran a test script `test_ai_plan.py` to call the `ai_plan_study_blocks` function locally with the database session. The test ran successfully without raising the `NameError`.
- Cleaned up the test script.

## UI Improvements Released (AI Plan Review Mode & Contrast Fixes)

1. **Better Contrast for Global Confirmation Modals**: Adjusted the styling of Confirm/Cancel buttons across the app to have clearer visual contrast against the background (e.g. solid white text on dark primary buttons).
2. **AI Plan Review Mode**: 
   - Instead of displaying a confusing "Review First" button inside a standard confirm dialog, clicking *AI Plan* now **previews** the generated study blocks directly on your timetable (using dashed outlines).
   - A floating smart action bar appears at the bottom allowing you to **Apply** or **Discard** the entire plan, so you can see exactly where the slots fit before committing!
   - **Clarity Update**: The AI plan blocks will now show the target assignment name (without the repetitive "Study: " prefix) along with the expected module code, making it instantly clear what you are studying for!

You can now test the **AI Plan** button in the Timetable UI to verify that it generates study blocks properly, previews them correctly without throwing errors, and let you apply them if everything looks good.
