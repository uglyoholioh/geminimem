# Fix Timetable Display During Non-Teaching Weeks

## Root Cause

Today (**Feb 26, 2026**) is **Recess Week** at NUS (Semester 2, AY2025-2026). The `slot_occurs_on_date` function in [timetable.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/timetable.py#L25-L114) correctly returns `False` for all slots during Recess Week — no classes occur.

However, the frontend uses the **same empty state** for "no timetable imported" and "no classes this week":

```tsx
// line 441 of page.tsx
!weekData || weekData.slots.length === 0 ? (
  // Shows "No class schedules yet" + import button
```

So after importing, the user sees the import prompt again, thinking nothing was saved.

> [!IMPORTANT]
> The import itself works correctly — data IS saved. The issue is purely a display/UX problem.

## Proposed Changes

### Backend

#### [MODIFY] [timetable.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/timetable.py)

Add a `total_slots` count to the `/week` endpoint response, querying the total number of slots for the user (regardless of week). This lets the frontend distinguish "no imports" from "no classes this week."

```diff
 return {
     "week_start": monday.isoformat(),
     "week_end": sunday.isoformat(),
     "academic_week": academic_info,
     "slots": week_slots,
+    "total_slots": len(all_slots),
 }
```

---

### Frontend

#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/timetable/page.tsx)

**1. Update `WeekData` type** to include `total_slots`.

**2. Differentiate empty states:**
- `total_slots === 0` → show import prompt ("No class schedules yet")
- `total_slots > 0 && slots.length === 0` → show empty grid with a Recess/non-teaching week banner

**3. After successful import, navigate to nearest teaching week** if current week is non-teaching:
- Check `academic_week.label` — if it's "Recess Week", "Reading Week", "Vacation", etc., advance `currentWeekStart` to skip to the next teaching week

**4. Add a banner** at the top of the timetable grid when the current week is a non-teaching week.

## Verification Plan

### Existing Tests

Run existing timetable tests:
```bash
cd /Users/oli/Desktop/CraftCanvas/backend && python -m pytest tests/test_routers/test_timetable.py -v
```

### Manual Verification

Since the fix is primarily about the recess week UX (which is happening right now in real time):

1. Start the backend: `cd /Users/oli/Desktop/CraftCanvas/backend && python -m uvicorn main:app --reload`
2. Start the frontend: `cd /Users/oli/Desktop/CraftCanvas/frontend && npm run dev`
3. Open the timetable page in the browser
4. Import a timetable (use the reference ICS file at `ReferenceFiles/nusmods_calendar (3).ics` or a NUSMods URL)
5. Verify that after import, the page navigates to a teaching week and shows classes
6. Navigate back to current week (Recess Week) and verify the banner message appears instead of the import prompt
