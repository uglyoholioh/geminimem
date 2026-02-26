# NUSMods API Integration Plan

## Goal Description
The current ICS-based timetable import relies on `RRULE` and `EXDATE`s, which can be fragile or imprecise for certain irregular modules (like `GESS1025`, which occurs on alternating weeks). 
The NUSMods API provides robust, structured data about every module, including precise teaching weeks, exam dates, workloads, and descriptions. This plan outlines how we can deeply integrate the NUSMods API into CraftCanvas for a better scheduling and planning experience.

## Proposed Features

### 1. Robust Timetable Syncing (The Fix for Irregular Classes)
Instead of relying strictly on ICS files, we can use the **module code** and **class number** (e.g. `GESS1025 Tutorial D30`) to fetch the exact schedule from the NUSMods API.
- **Accurate Weeks Array**: NUSMods provides an explicit array of weeks a class occurs (e.g. `[3, 5, 7, 9, 11, 13]`). 
- **Academic Week Integration**: We can map this directly to our existing `get_academic_week` function. If the current teaching week is in the array, the class displays. This guarantees 100% accuracy.
- **Import via Share Link**: We can add a feature to import a timetable simply by pasting the NUSMods Share URL (e.g. `https://nusmods.com/timetable/sem-2/share?BT1101=LEC:1,TUT:4,LAB:0...`). No need to download `.ics` files!

### 2. Exam Integration
The NUSMods API provides exact exam dates and durations for each module (e.g., `examDate: "2026-04-28T13:00:00.000Z"`).
- We can automatically populate the user's planner/calendar with their final exams as soon as they import their timetable or add a course.

### 3. Rich Module Briefs & Information
The API provides comprehensive data about the module itself:
- `title` & `description`
- `moduleCredit` & `workload` (e.g., `[2, 1, 0, 3, 4]` — hours for Lecture, Tutorial, Lab, Project, Prep)
- `prerequisite`, `corequisite`, and `preclusion` information.
- We can display this info in the "Courses" tab or in the Daily Brief. It provides the AI with richer context when summarizing or helping with tasks for that module.

## Implementation Steps

### Phase 1: API Client & Week Array Logic (Your immediate fix)
1. **[NEW] `backend/services/nusmods.py`**: Create a service to fetch from `https://api.nusmods.com/v2/{year}/modules/{code}.json`.
2. **[MODIFY] `backend/models/timetable.py`**: Add a `weeks` array column (e.g., `JSON` or comma-separated string `3,5,7,9`) to `TimetableSlot`.
3. **[MODIFY] `backend/routers/timetable.py`**: 
   - Update the `/import` endpoint. After parsing the ICS file, fetch the module from the NUSMods API, find the matching lesson by `classNo` and `lessonType`, and store its `weeks` array.
   - Update `slot_occurs_on_date` to check if the current teaching week from `get_academic_week_info` matches the stored `weeks` array instead of using RRULEs.

### Phase 2: Share Link Import (Quality of Life)
1. **[MODIFY] `backend/routers/timetable.py`**: Add a new `/import-url` endpoint that takes a NUSMods Share URL, parses the query parameters, fetches the data from the NUSMods API, and creates `TimetableSlot`s directly (bypassing ICS entirely).
2. **[MODIFY] `frontend/app/timetable/page.tsx`**: Add an input field to paste the URL next to the existing ICS upload button.

### Phase 3: Exams & Rich Data
1. **[MODIFY] `backend/models/course.py`**: Add fields for `credits`, `description`, `exam_date`.
2. **[MODIFY] `frontend/app/courses/[id]/page.tsx`**: Display the module description, workload, and exam date in the course details header.


### Phase 4: Timetable Accuracy & Week Offset Fix
1. **[MODIFY] `backend/routers/timetable.py`**: Correct the hardcoded semester start dates in `get_academic_week_info`. Specifically, change `AY2025/26 Sem 2` from `13 Jan 2026` to `12 Jan 2026`.
2. **[VERIFY] teaching_week mapping**: Ensure the `raw_week` to `teaching_week` conversion (raw_week - 1 after recess) correctly aligns with the NUSMods `weeks` array for modules like `GESS1025`.

## User Review Required
> [!IMPORTANT]
> The timetable "breakage" reported is likely due to a 1-day offset in the hardcoded semester start date (Jan 13 instead of Jan 12). This causes Monday classes to be mislabeled and shifts the entire academic week calculation. I will fix this immediately.
