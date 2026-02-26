# Timetable Import Display Fix — Walkthrough

## Root Cause

Today (Feb 26, 2026) is **NUS Recess Week**. The `slot_occurs_on_date` function correctly filters out all classes during recess. However, the frontend showed the same "No class schedules yet" empty state whether classes were imported or not — making the import appear broken.

## Changes Made

### Backend — [timetable.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/timetable.py)

Added `total_slots` to the `/week` response so the frontend can distinguish "no imports" from "no classes this week":

```diff
 return {
     ...
     "slots": week_slots,
+    "total_slots": len(all_slots),
 }
```

---

### Frontend — [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/timetable/page.tsx)

Three changes:

1. **Differentiated empty states** — `total_slots === 0` shows the import prompt; `total_slots > 0 && slots === 0` shows a contextual banner with a ☕ Coffee icon and a message specific to the week type (Recess, Reading, Exam, etc.)

2. **Auto-navigate after import** — `navigateToTeachingWeek(data)` jumps to semester week 1 if the current week is non-teaching, so users immediately see their imported classes

3. **Fixed stale closure** — `fetchWeek` now returns the data directly instead of relying on React state, which avoids the async state update timing issue

## Verification

| Check | Result |
|---|---|
| Backend tests (`test_timetable.py`) | ✅ 2/2 passed |
| Frontend build (`next build`) | ✅ Clean build |
