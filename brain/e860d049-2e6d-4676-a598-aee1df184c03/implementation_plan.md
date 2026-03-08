# Course Data Drill-down Implementation Plan

The goal is to allow users to "drill down" into all Canvas data for a module, specifically Modules, Pages, Files, and the Syllabus.

## Proposed Changes

### Backend

#### [MODIFY] [schemas/course.py](file:///Users/oli/Desktop/CraftCanvas/backend/schemas/course.py)
Add new nested schemas for:
- `CanvasModuleNested`
- `CanvasModuleItemNested`
- `CanvasPageNested`
- `CanvasFileNested`
Update `CourseDetail` to include:
- `modules: List[CanvasModuleNested]`
- `pages: List[CanvasPageNested]`
- `files: List[CanvasFileNested]`
- `syllabus_body: Optional[str]`

#### [MODIFY] [routers/courses.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/courses.py)
Update `get_course` endpoint to:
- Fetch `CanvasModule` and related `CanvasModuleItem`s.
- Fetch `CanvasPage`s for the course.
- Fetch `CanvasFile`s for the course.
- Include `syllabus_body` in the response (it's already in the model).

### Frontend

#### [MODIFY] [[id]/page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/courses/[id]/page.tsx)
- Update `CourseDetail` interface.
- Add new `TabId` types: `modules`, `pages`, `files`, `syllabus`.
- Implement new tabs in the navigation and main content area.
- Create components for:
  - `ModulesTab`: Collapsible module list with items.
  - `PagesTab`: List of wiki pages with a simple text viewer.
  - `FilesTab`: List of files organized by folder path.
  - `SyllabusTab`: HTML renderer for the syllabus body.

---

## Verification Plan

### Automated Tests
- Run backend tests to ensure `get_course` returns the new data correctly.
- `pytest backend/tests/test_routers/test_courses.py` (if it exists, otherwise I'll check `test_canvas_sync.py` and potentially add a router test).

### Manual Verification
1.  Navigate to a course detail page.
2.  Switch to the "Modules" tab and verify the structure matches Canvas.
3.  Switch to "Pages" and view a wiki page's content.
4.  Switch to "Files" and verify folder paths and file metadata.
5.  Switch to "Syllabus" and verify the syllabus text is displayed.
