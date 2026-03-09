# Implementation Plan - Course Content Organization

The current "Files" and "Modules" tabs on the course detail page are unorganised. The "Files" tab displays a flat list of all files, while the "Modules" tab strictly follows the Canvas module structure. This plan aims to organize this content more logically and give users control over how they view it.

## Proposed Changes

### Frontend Enhancement

#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/courses/[id]/page.tsx)

1.  **Introduce View Toggle for Files**:
    *   Add a toggle in the "Files" tab to switch between:
        *   **Canvas Folders View**: Tree-like structure based on `folder_path`.
        *   **Date View**: Grouped by upload date (yesterday, last week, etc.).
        *   **Type View**: Grouped by file type (PDFs, Images, etc.).
2.  **Enhance Modules Tab**:
    *   Add a search/filter bar to find specific items within modules.
    *   Implement "Expand/Collapse All" for modules.
3.  **Cross-Reference Files**:
    *   When viewing a file that is also part of a module, show a "Part of Module: [Name]" badge.
4.  **UI/UX Refinements**:
    *   Use a more "premium" file browser UI with icons, breadcrumbs for folder navigation, and better empty states.

### Backend Changes

No backend changes are strictly required as the current `get_course` endpoint returns all necessary data (`modules`, `items`, `files`, `pages`) with their metadata (`folder_path`, `type`, etc.). The organization will be handled on the frontend for maximum flexibility.

## Verification Plan

### Automated Tests
*   **Jest/React Testing Library**:
    *   Verify that toggling the view mode correctly re-renders the file list.
    *   Verify that the folder navigation correctly filters files.
*   **E2E (Playwright)**:
    *   Navigate to a course detail page.
    *   Switch to "Files" tab.
    *   Verify default view (Canvas Folders).
    *   Switch to "Date" view and verify grouping.

### Manual Verification
*   Open the app in the browser.
*   Go to a course with many files and modules.
*   Interact with the new view toggles and folder navigation.
*   Ensure smooth transitions and no layout shifts.
