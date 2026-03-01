# Implementation Plan - Module Hub Improvements

The goal is to enhance the Module Hub (dashboard and courses page) to be more visually appealing ("premium"), provide better at-a-glance information, and improve the user experience for managing modules.

## User Review Required

> [!IMPORTANT]
> I will be consolidating the Module Hub card design. The dashboard cards will remain more compact than the courses page cards, but they will share a unified aesthetic.

## Proposed Changes

### Dashboard

#### [MODIFY] [ModuleHub.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/ModuleHub.tsx)
- **Visuals**: Enhance the card design with better glassmorphism (subtle border glows, refined backdrop-blur).
- **Content**:
    - Update the normal view to show the module name below the code (using truncated text).
    - Refine the hover overlay to be less intrusive or more informative.
    - Implement a "Quick Stats" row on hover that shows specific counts (e.g., "2 Due Today").
- **UX**: Rename "VISIBILITY" to "MANAGE" and improve the toggle animation/feedback.

### Courses Page

#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/courses/page.tsx)
- **Visuals**: Match the new "premium" aesthetic of the dashboard.
- **Content**: 
    - Add "Rich Metadata" display (e.g., Credits, Exam Date) to the `CourseCard`.
    - Improve the layout for better information density.
- **Interaction**: Change "Hide/Show" to "Pin to Dashboard" for better clarity on what the toggle does.

### Shared / Library

#### [MODIFY] [types.ts](file:///Users/oli/Desktop/CraftCanvas/frontend/lib/types.ts)
- Ensure all rich metadata fields are available in the frontend `Course` type.

---

## Verification Plan

### Automated Tests
- Run `npm run test` to ensure no regressions in existing dashboard components.
- Check for build errors with `npm run build` (or `next build`).

### Manual Verification
1.  **Dashboard**:
    - Verify that module cards show the module name.
    - Verify hover effects and quick actions.
    - Test the "Manage" (formerly Visibility) toggle to ensure modules can be hidden/shown correctly.
2.  **Courses Page**:
    - Verify the updated `CourseCard` design.
    - Verify that "Pin to Dashboard" correctly reflects the `is_active` status.
    - Check if rich metadata (like credits) displays correctly if available.
