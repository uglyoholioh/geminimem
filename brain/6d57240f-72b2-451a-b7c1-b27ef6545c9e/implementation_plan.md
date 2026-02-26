# Dashboard Visual Alignment Plan

Align the dashboard with the "Editorial" newspaper aesthetic seen in the Planner, optimize the bento grid layout to remove blank space, and implement content truncation for density.

## Proposed Changes

### Dashboard Layout
#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/LMSManager/frontend/app/page.tsx)
- Refactor the grid layout to be tighter and more intentional.
- Standardize block containers with the editorial `border-2 border-black` (light) / `border-neutral-600` (dark) style.
- Use serif fonts for section headers and Mono for metadata/stats.
- Ensure blocks "fit together" by using spanning columns/rows effectively.

### Components
#### [MODIFY] [greeting-card.tsx](file:///Users/oli/Desktop/LMSManager/frontend/components/dashboard/greeting-card.tsx)
- Align header style with the Planner's horizontal compact header.
- Use bold serif titles and thin mono accents.

#### [MODIFY] [daily-brief.tsx](file:///Users/oli/Desktop/LMSManager/frontend/components/dashboard/daily-brief.tsx)
- Implement compact styling.
- Add a "max-height" and "view full" link to prevent the brief from taking up too much vertical space.
- Remove redundant padding and inner cards where possible to maximize density.

#### [MODIFY] [today-schedule.tsx](file:///Users/oli/Desktop/LMSManager/frontend/components/dashboard/today-schedule.tsx)
- Align with the editorial theme (bold borders, serif headers).

## Verification Plan

### Automated Tests
- Perform visual regression check (manual) in browser.
- Verify responsive layout on mobile vs desktop.

### Manual Verification
- Check Light and Dark mode contrast and consistency.
- Verify Content Cutoff works correctly for long Daily Briefs.
- Confirm "View all" links are correctly positioned and styled.
