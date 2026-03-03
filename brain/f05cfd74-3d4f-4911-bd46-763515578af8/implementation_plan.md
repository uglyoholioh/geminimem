# Timetable Text Cutoff Adjustments

The goal is to ensure that timetable event blocks remain readable and informative even when space is limited (e.g., short-duration events or narrow columns). Currently, fixed constraints and `overflow-hidden` cause text to be cut off without a way for the user to see the full content.

## Proposed Changes

### Timetable Component

#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/timetable/page.tsx)

1.  **Add Hover Tooltips**:
    *   Add a `title` attribute to each event block `div` containing the full title/module code, lesson type, venue, and time. This provides immediate, low-cost "accommodation" for cutoff text.
2.  **Enhance Block Layout for Small Sizes**:
    *   **Vertical View**:
        *   If `spanHrs < 1.1` (e.g., 1-hour slots), reduce padding and gap size.
        *   If `spanHrs < 1.0`, hide the module name/description by default and only show it on hover or if there's enough room.
        *   Ensure the time and venue info use a smaller font if space is tight.
    *   **Horizontal View**:
        *   Adjust the `line-clamp` logic. For events less than 2 hours wide, use `line-clamp-1` or smaller font sizes.
        *   Use `truncate` on the module code/title span to ensure it shows ellipsis.
3.  **Interactive Hover Expansion**:
    *   Add a `group-hover:z-20` and potentially a subtle scale or shadow effect to make the hovered block stand out.
    *   Consider allowing the block to "expand" over neighboring empty space on hover if text is truncated.

## Verification Plan

### Automated Tests
- Run existing E2E tests to ensure no regressions in timetable functionality.
```bash
cd frontend && npx playwright test e2e/verification.spec.ts
```

### Manual Verification
1.  **Horizontal View**:
    *   Verify that 1-hour blocks show an ellipsis if the title/module name is too long.
    *   Verify that hovering over a block shows the full details in a native tooltip.
    *   Check that narrow blocks still display the most essential information (Module Code/Category).
2.  **Vertical View**:
    *   Verify that 1-hour (60px) slots still show the module code and time.
    *   Check for overlapping text in 30-minute or 1-hour slots.
3.  **Dark/Light Mode**:
    *   Ensure hover states and tooltips remain readable in both modes.
