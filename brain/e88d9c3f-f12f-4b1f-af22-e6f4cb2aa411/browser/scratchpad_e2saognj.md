# Timetable Layout Inspection

## Plan
1. [x] Navigate to http://localhost:3000/timetable
2. [x] Observe the layout, especially CS2030 Recitation
3. [x] Check horizontal and vertical views
4. [x] Take screenshots and identify issues
    - [x] Uneven rows
    - [x] Slots bleeding into other hours
    - [x] Text overflowing
5. [x] Report findings

## Observations
- **Horizontal View**:
    - Day rows have inconsistent heights. The Wednesday row is very tall due to CS2030 Recitation expansion.
    - Text truncation still occurs in some slots (e.g., BT1101) even when they could potentially expand.
    - Vertical expansion of a slot pushes the entire day's row height, which is expected for a grid but looks "uneven".
- **Vertical View**:
    - The time axis is non-linear. The 13:00-14:00 row is much taller than other hour rows because of the CS2030 Recitation on Wednesday.
    - This makes 1-hour slots appear much larger than they should be relative to other slots, which the user likely describes as "expanding outside of their own zone".
    - Slots that span multiple hours (e.g., CS2030 LEC on Monday 12-14) have distorted shapes because one of the hours (13:00) is taller than the other (12:00).
    - Empty hours (like 15:00) are very squashed, while busy hours are tall.
- **User's "Exact Opposite" Feedback**:
    - The user wants slots to expand vertically to show text, but "stay in their slot" and "stay within its grid".
    - Currently, expansion in the Vertical view distorts the entire timeline, which violates the "zone" of the time slot visually.
    - In Horizontal view, it's better, but truncation is still happening for some modules.
