# Implementation Plan - Remove Grades Page and Widget

This plan outlines the steps to completely remove the grades feature from the Academic Life OS, including the frontend page, dashboard widget, backend API, and AI tool integration.

## Proposed Changes

### Backend

#### [DELETE] [grades.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/grades.py)
- Delete the router file.

#### [DELETE] [grade.py](file:///Users/oli/Desktop/CraftCanvas/backend/models/grade.py)
- Delete the model file.

#### [MODIFY] [main.py](file:///Users/oli/Desktop/CraftCanvas/backend/main.py)
- Remove `from routers.grades import router as grades_router` (line 25).
- Remove `app.include_router(grades_router, ...)` (line 105).

#### [MODIFY] [ai_tools.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_tools.py)
- Remove `from models.grade import GradeComponent, GradeEntry` (line 15).
- Remove `get_grades` function.
- Remove `get_grades` from `TOOL_DEFINITIONS`.

---

### Frontend

#### [DELETE] [grades](file:///Users/oli/Desktop/CraftCanvas/frontend/app/grades)
- Delete the entire directory containing the grades page.

#### [DELETE] [GradesSnapshot.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/widgets/GradesSnapshot.tsx)
- Delete the widget component.

#### [MODIFY] [Sidebar.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/layout/Sidebar.tsx)
- Remove the grades link: `{ href: '/grades', icon: BarChart3, label: 'Grades' }`.

#### [MODIFY] [DashboardLayoutManager.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/DashboardLayoutManager.tsx)
- Remove the `case 'grades_snapshot':` in the widget rendering logic.

#### [MODIFY] [widgetRegistry.ts](file:///Users/oli/Desktop/CraftCanvas/frontend/lib/widgetRegistry.ts)
- Remove the `grades_snapshot` object from `WIDGET_REGISTRY`.

---

### Documentation

#### [MODIFY] [ARCHITECTURE.md](file:///Users/oli/Desktop/CraftCanvas/docs/ARCHITECTURE.md)
#### [MODIFY] [BUILD_PLAN.md](file:///Users/oli/Desktop/CraftCanvas/docs/BUILD_PLAN.md)
#### [MODIFY] [FEATURES.md](file:///Users/oli/Desktop/CraftCanvas/docs/FEATURES.md)
#### [MODIFY] [DATA_SCHEMA.md](file:///Users/oli/Desktop/CraftCanvas/docs/DATA_SCHEMA.md)
- Remove all mentions and descriptions of the "Grades" feature.

## Verification Plan

### Automated Tests
- Run `npm run build` in the `frontend` directory to ensure no broken references remain.
- Start the backend and ensure it runs without errors.

### Manual Verification
- Verify that the "Grades" link is gone from the sidebar.
- Verify that navigating to `/grades` manually results in a 404.
- Verify that the dashboard still loads and doesn't crash if a `grades_snapshot` widget was previously saved in the layout.
