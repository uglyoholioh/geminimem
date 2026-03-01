# Implementation Plan - Font Size Optimization

## Goal
Increase font sizes across the application to improve legibility and accessibility. Many elements currently use font sizes between 9px and 11px, which are too small for comfortable reading. The goal is to bring most metadata to at least 12px (`text-xs`) and primary content/labels to at least 14px (`text-sm`).

## Proposed Changes

### [Timetable]
#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/timetable/page.tsx)
- Increase time labels from `text-[10px]` to `text-xs` (12px).
- Increase module type badges from `text-[9px]` to `text-xs` (12px).
- Increase module names from `text-[11px]` to `text-sm` (14px).
- Increase metadata (Venue, Clock) from `text-[10px]` to `text-xs` (12px).
- Scale icons accordingly to match the new font sizes.

### [Dashboard]
#### [MODIFY] [AgendaTimeline.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/AgendaTimeline.tsx)
- Increase header and secondary labels from `text-[10px]` or `text-[11px]` to `text-xs` (12px).
- Increase item titles from `text-[11px]` to `text-sm` (14px).
- Increase badges from `text-[9px]` to `text-xs` (12px).

#### [MODIFY] [ModuleHub.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/ModuleHub.tsx)
- Increase header and button text from `text-[10px]` or `text-[11px]` to `text-xs` (12px).
- Increase module codes in manage view from `text-[9px]` to `text-xs` (12px).
- Increase module info in cards from `text-[11px]` to `text-sm` (14px).

### [Course Detail]
#### [MODIFY] [[id]/page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/courses/[id]/page.tsx)
- Increase stats labels (Credits, Workload, etc.) from `text-[10px]` to `text-xs` (12px).
- Increase badges from `text-[9px]` to `text-xs` (12px).
- Increase secondary metadata from `text-[10px]` to `text-xs` (12px).

## Verification Plan

### Automated Tests
- None planned as this is a visual change.

### Manual Verification
1. Run the app locally (`npm run dev` in `frontend`).
2. Navigate to the Dashboard, Timetable, and Course Detail pages.
3. Verify that the font sizes are noticeably larger and more readable.
4. Check that layouts remain intact and don't break with the slightly larger text.
5. Use browser developer tools to confirm that the new font sizes are at least 12px for metadata and 14px for content.
