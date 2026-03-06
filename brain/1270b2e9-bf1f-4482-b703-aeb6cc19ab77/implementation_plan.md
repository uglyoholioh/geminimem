# Implementation Plan - Fix Focus Page Layout Cropping

The "weirdly cropped" Focus page is caused by a fixed `max-h-[90vh]` on the main container card, which exceeds the available viewport height on many screens when accounting for the header height and parent padding. The parent container also has `overflow-hidden`, which prevents scrolling to see the clipped parts.

## Proposed Changes

### [Component Name] Focus Page

#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/focus/page.tsx)

- Update the outer container (line 656) to allow vertical overflow if the screen is extremely small, but keep it clean by default.
- Change the main card's `max-h-[90vh]` (line 696) to `max-h-full` to ensure it respects the parent's boundaries and padding.
- Ensure the inner columns correctly handle scrolling when the card height is constrained by the viewport.

```diff
- <div className="flex-1 flex items-center justify-center p-6 lg:p-12 relative overflow-hidden">
+ <div className="flex-1 flex items-center justify-center p-6 lg:p-12 relative overflow-y-auto">

...

- <div className={`w-full ${enableSpotify ? 'max-w-[95vw] xl:max-w-[1400px]' : 'max-w-5xl'} max-h-[90vh] ...
+ <div className={`w-full ${enableSpotify ? 'max-w-[95vw] xl:max-w-[1400px]' : 'max-w-5xl'} max-h-full ...
```

## Verification Plan

### Automated Tests
- None applicable for layout issues, but will check build status.

### Manual Verification
1. Run the frontend: `npm run dev` in `frontend/`.
2. Navigate to `/focus`.
3. Resize the browser window to various heights (tall and short).
4. Verify that the Focus card fits within the viewport and the tasks/middle section remains scrollable without the card edges being clipped by the screen.
5. Confirm "6 Issues" toast doesn't block critical controls.
