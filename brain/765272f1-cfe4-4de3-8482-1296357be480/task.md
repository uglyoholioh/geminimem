# Timetable Accuracy Fix Task List

## Phase 4: Week Offset & Labeling Fix
- [x] Correct Jan 2026 start date to Jan 12 (Monday)
- [x] Fix 2024/25 Sem 2 start date to Jan 13 (Monday) (No change needed, already Jan 13)
- [x] Verify GESS1025 occurrence on odd weeks 3, 5, 7, 9, 11, 13
- [x] Ensure multiple lessons (e.g. MA1522 LEC2) appear on both days
- [x] Confirm Recess Week labeling and data exclusion

## Phase 5: ICS Import Verification
- [x] Fix UnboundLocalError bug in `import_timetable`
- [x] Import `nusmods_calendar (3).ics` via API
- [x] Verify slots for Week 1 (Classes visible)
- [x] Verify slots for Recess Week (Empty, labeled)
- [x] Verify alternating week logic (GESS1025 visible Week 7, hidden Week 6)
- [x] Verify multi-slot duplication (MA1522 appearing twice per week)

## Phase 6: Index-Based URL Parser Fix
- [x] Identify non-standard NUSMods URL format (`LEC:(0);LAB:(10)`)
- [x] Update `parse_nusmods_url` to support index-based extraction
- [x] Update `/import-url` endpoint to match classes by array index
