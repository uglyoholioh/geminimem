# Implementation Plan - Add Smaller Widget Size for Upcoming Agenda

The goal is to provide a more compact version of the "Upcoming Agenda" widget that fits into a single column on the dashboard.

## Proposed Changes

### [Component Name] Dashboard
#### [MODIFY] [widgetRegistry.ts](file:///Users/oli/Desktop/CraftCanvas/frontend/lib/widgetRegistry.ts)
- Add `'sm'` to `supportedSizes` for the `agenda` widget.

#### [MODIFY] [AgendaTimeline.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/AgendaTimeline.tsx)
- Update `AgendaTimelineProps` to correctly handle the `size` prop.
- Implement conditional styling and layout for the `sm` size:
    - Reduce horizontal padding from `px-4` to `px-2` (or similar).
    - Compact the time display area.
    - Adjust the timeline line position.
    - Reduce padding in the individual item cards.
    - More aggressive line clamping for titles.
    - Potentially hide less critical information (like venue) in the `sm` view to prioritize readability.

## Verification Plan

### Manual Verification
1.  Open the dashboard.
2.  Enter edit mode.
3.  Change the "Upcoming Agenda" widget size to `SM`.
4.  Verify that the widget now occupies a single column.
5.  Verify that the layout remains readable and looks aesthetic in the smaller size.
6.  Switch back to `MD` and `LG` to ensure no regressions in the larger views.
