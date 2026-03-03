# AI Plan Improvements

## Phase 1: Backend — Smarter Scheduling Engine
- [x] Refactor `ai_plan_study_blocks` in `backend/routers/timetable/main.py`
  - [x] Add 30-min buffer after classes
  - [x] Add user preference support (study hours, session duration)
  - [x] Balance workload across days (max sessions/day cap)
  - [x] Allow multiple sessions for assignments due far away
  - [x] Configurable preferred hours, session duration, max daily (from settings)

## Phase 2: Frontend — Preview Bar & Visual Improvements
- [x] Show per-block module code breakdown in the preview bar
- [x] Add pulsing dashed border for preview blocks (vertical view)
- [x] Add pulsing dashed border for preview blocks (horizontal view)

## Phase 3: Verification
- [x] Backend syntax check
- [ ] Manual test the full flow
