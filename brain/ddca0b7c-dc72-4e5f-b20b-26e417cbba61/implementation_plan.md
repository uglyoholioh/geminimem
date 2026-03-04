# Implement Resizable Multi-Size Widgets

This plan introduces a CSS Grid-based widget system that supports different widget sizes ('sm', 'md', 'lg'), similar to iOS widgets. 

## Proposed Changes

### 1. `frontend/lib/widgetRegistry.ts`
- **[MODIFY]** Update `WidgetLayout` to include a `size?: 'sm' | 'md' | 'lg'` property.
- **[MODIFY]** Update `WidgetDefinition` to include `supportedSizes?: ('sm' | 'md' | 'lg')[]` and a default size enum (`sm`, `md`, `lg`) instead of the current `{ w: 1|2, h: number }`.

### 2. `frontend/components/dashboard/DashboardLayoutManager.tsx`
- **[MODIFY]** Change the wrapper from `flex flex-col` to a proper grid: `grid grid-cols-2 gap-4 auto-rows-min`.
- **[MODIFY]** Pass an `onResize` callback to `WidgetShell` so that widgets can update their size in the configuration.
- **[MODIFY]** Provide the current `size` down to each `renderWidget` call so widgets can adapt their UI.

### 3. `frontend/components/dashboard/WidgetShell.tsx`
- **[MODIFY]** Accept `size` and `onResize` props.
- **[MODIFY]** Apply dynamic Tailwind CSS grid sizing classes (`col-span-1`, `col-span-2`, `row-span-1`, `row-span-2`) based on the widget's size.
- **[MODIFY]** Add a size selector UI (like a small dropdown or toggle icons) in the widget header when `isEditing` is true.

### 4. `frontend/components/dashboard/widgets/*`
- **[MODIFY]** Update existing widgets to accept a `size` prop and alter their rendering (e.g., hide/show extra details, change typography scales) based on the chosen size. Examples include `FocusMini.tsx`, `PinnedNotes.tsx`, `SemesterCountdown.tsx`, etc.

## Verification Plan

### Automated Tests
- Run the Playwright end-to-end tests: `npx playwright test e2e/verification.spec.ts`

### Manual Verification
- Go to the Dashboard and enter Edit Mode.
- Add various widgets and test the new size selector in the header.
- Verify that resizing a widget correctly reflows the grid.
- Verify that widgets display a different UI based on their size (e.g., `sm` vs `lg` views).
- Check that layout preferences save on drop/resize and persist after a page refresh.
