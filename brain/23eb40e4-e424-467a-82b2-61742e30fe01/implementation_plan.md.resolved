# Update Timetable Bottom Section

The user requested that the "Modules" section at the bottom of the timetable page be replaced with something more useful. Since the Stitch project creation tool was repeatedly canceled, I am proposing a "Weekly Insights" design as an alternative.

## Proposed Changes

### [frontend/app/timetable/page.tsx]

#### Remove "Modules" Section
- Remove the existing `div` that renders the `uniqueModules` map at the bottom of the page.

#### Add "Weekly Insights" Section
- We will add a new `useMemo` block to calculate some weekly statistics based on the `weekData.slots`.
- We will compute insights such as:
  - **Total Contact Hours:** The sum of durations for all classes in the current week.
  - **Busiest Day:** The day with the most scheduled contact minutes.
  - **Skipped Classes:** The count of slots where `is_attended === false`.
  - **Modules:** The number of unique modules scheduled for the selected week.

#### Render Changes
- We will render these 4 insights as sleek cards/pills at the bottom of the page in place of the old module legend, providing more actionable and interesting data for the student at a glance.

## User Action Required
> [!IMPORTANT]
> The Stitch tool kept throwing a cancellation error, so I couldn't use it to generate the design automatically. 
> Does this "Weekly Insights" idea sound like a good use of that space? If you approve, I will proceed with the implementation!
