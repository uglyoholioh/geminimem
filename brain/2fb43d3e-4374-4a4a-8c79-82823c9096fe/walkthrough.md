# AI Plan Improvements — Walkthrough

## Backend: Smarter Scheduling Engine

Refactored [main.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/timetable/main.py) `ai_plan_study_blocks`:

| Feature | Before | After |
|---|---|---|
| **Buffer after classes** | None — could stack study right after a lecture | 30-min gap after every class |
| **Preferred hours** | Hardcoded `[9, 11, 14, 16, 19]` | Reads `ai_plan_preferred_hours` setting; falls back to defaults |
| **Session duration** | Always 2 hours | Reads `ai_plan_session_duration` (0.5–4 hrs); default 2 |
| **Daily cap** | Unlimited | Reads `ai_plan_max_daily` (1–6); default 3 |
| **Multi-session** | 1 block per assignment | 2 blocks for assignments due >3 days away, spaced across days |
| **Block limit** | 10 | 12 |
| **Collision detection** | Multiple manual loops with hour-level math | Clean `_get_busy_intervals` + `_has_collision` using minute-level precision |

Settings are stored in the existing key-value `Settings` table — no migration needed.

---

## Frontend: Preview Bar & Visual Enhancements

Updated [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/timetable/page.tsx):

1. **Module breakdown in preview bar**: Shows `"4 blocks · CS2103T ×2, MA1522 ×1"` instead of just `"4 suggested blocks"`.
2. **Pulsing dashed border**: Preview blocks now render with `border-dashed border-2 animate-pulse` and a lighter translucent background so they visually stand out from real events in both vertical and horizontal views.

---

## Verification

- ✅ Backend Python syntax check passed
- ✅ No new TS lint errors introduced in frontend
- Manual testing recommended: click AI Plan → verify preview blocks appear with pulsing style, review the module breakdown bar, apply/discard
