# Timetable Visibility Toggle Restoration

I have restored the visibility toggle for classes that are marked as "unattended" (skipped) in the timetable.

## Key Changes

### Timetable Header
- Added a new **Skipping** toggle button in the header.
- The button uses an `Eye`/`EyeOff` icon to indicate its state.
- When active, classes marked as unattended are hidden from the grid.
- The state of this toggle is saved to your browser's local storage, so it persists across refreshes.

### Timetable Grid
- Updated both **Horizontal** and **Vertical** view modes to respect the `hideUnattended` setting.
- Toggle logic is applied per-slot, allowing for dynamic filtering of the schedule.

## Demo

![Visibility Toggle in Header](/Users/oli/Desktop/CraftCanvas/frontend/public/images/timetable_visibility_toggle.png)
*Iconography and state management for the new visibility toggle.*

## Verification Results

- [x] **Unattended Visibility**: Manually toggled individual classes to "unattended" and verified they fade out.
- [x] **Global Toggle**: Verified that clicking the "Skipping" toggle hides/shows all unattended classes.
- [x] **Persistence**: Verified that the toggle state is remembered after a page refresh.
- [x] **Layout Integrity**: Verified that the grid layout remains stable in both vertical and horizontal views when classes are hidden.
