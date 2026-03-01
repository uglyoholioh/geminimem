# Implementation Plan - Agenda Improvements

Revitalize the "Today's Agenda" component to provide a more comprehensive, interactive, and visually stunning overview of the user's day and week.

## Proposed Changes

### Bug Fix: Incorrect Date Grouping
- **Issue**: Sunday view shows Monday's classes as "Today".
- **Cause**: Frontend uses only `start_time` (HH:mm) and defaults to current date if ISO datestring is missing.
- **Fix**: Use the `start` ISO field from the backend and update the `TimetableSlot` interface to support it.

### Frontend Components

#### [MODIFY] [types.ts](file:///Users/oli/Desktop/CraftCanvas/frontend/lib/types.ts)
- Add `start?: string` and `end?: string` to `TimetableSlot`.

#### [MODIFY] [AgendaTimeline.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/AgendaTimeline.tsx)
- **Visual Overhaul**: Implement a more dynamic timeline with better color coding and icons.
- **Current Time Indicator**: Add a horizontal line representing the current time to help users orient themselves.
- **Rich Data**: Display more information such as duration, venue details, and status icons for tasks/assignments.
- **Multi-day Support**: Group items by date when the lookahead is more than one day.
- **Interactivity**: Add hover effects and links to detail pages.

#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/page.tsx)
- **Reactive Data**: Update `agendaItems` calculation to respect the `activeLookahead` state.
- **Enhanced Sorting/Filtering**: Ensure items across multiple days are correctly sorted and grouped.

### Aesthetics & UX
- Use `framer-motion` for smooth list transitions (if available, otherwise CSS transitions).
- Implement a "Glassmorphism" effect for card backgrounds.
- Use font weights and colors to distinguish between "Classes" (high priority), "Assignments" (deadlines), and "Tasks" (action items).

## Verification Plan

### Automated Tests
- N/A (UI focused)

### Manual Verification
- Verify that selecting "3 DAYS" or "WEEK" in the Command Center updates the Agenda Timeline.
- Check the "Current Time" indicator accurately reflects the system time.
- Verify responsiveness on mobile/tablet widths.
- Ensure empty states are gracefully handled.
