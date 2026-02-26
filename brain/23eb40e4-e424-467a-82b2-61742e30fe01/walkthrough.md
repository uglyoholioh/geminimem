# Timetable Weekly Insights Walkthrough

## Overview

The "Modules" legend at the bottom of the timetable page has been completely replaced with a more dynamic and actionable **Weekly Insights** section. This new section provides an at-a-glance summary of important statistics for the currently selected week.

## Changes Made

1. **Calculated Weekly Statistics (`weeklyInsights` useMemo Hook):**
   - We calculate 4 main data points from `weekData.slots`:
     - **Contact Hours:** The total duration (in hours and minutes) of all classes scheduled for the week.
     - **Busiest Day:** The specific day (e.g., "Monday") with the most scheduled contact time, along with the hours on that day.
     - **Skipped Classes:** The number of classes where the user has toggled attendance off (`is_attended === false`).
     - **Active Modules:** The count of unique modules that have at least one scheduled class during the week.

2. **Added Sleek UI Cards (`frontend/app/timetable/page.tsx`):**
   - The insights are displayed in a responsive grid of 4 cards at the bottom of the page.
   - The cards use styling from the design system, with a special state for "Skipped Classes". If the user skips a class, the "Skipped Classes" card turns slightly red (using the urgency color variable) to serve as a subtle warning.

## Verification

The code changes are mostly straightforward and depend primarily on `weekData.slots`.

- I recommend visually inspecting the timetable page by navigating through different weeks.
- Verify that the total contact hours appear accurate for a given week.
- Verify that toggling a class's attendance via the eye icon immediately updates the "Skipped Classes" counter. 

## Code Diff

```diff
render_diffs(file:///Users/oli/Desktop/CraftCanvas/frontend/app/timetable/page.tsx)
```
