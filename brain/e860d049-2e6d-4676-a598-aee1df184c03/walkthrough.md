# Course Data Drilldown - Enhanced Canvas Integration

This update enables a detailed "drill-down" into all Canvas data for each course module. Users can now access Modules, Pages, Files, and the Syllabus directly within CraftCanvas.

## Key Changes

### Backend
- **New Schemas**: Added Pydantic schemas for `CanvasModule`, `CanvasModuleItem`, `CanvasPage`, and `CanvasFile` in `backend/schemas/course.py`.
- **API Updates**: Enhanced the `get_course` endpoint in `backend/routers/courses.py` to fetch and return the complete hierarchy of module data.

### Frontend
- **Refactored Module Detail Page**: The `ModuleDetailPage` (`frontend/app/courses/[id]/page.tsx`) now features a more robust tab system.
- **New Tabs**:
    - **Modules**: Displays the full module structure with collapsible sections.
    - **Pages**: Accessible list of all course pages with content extracts.
    - **Files**: Complete file management view with download links and file metadata.
    - **Syllabus**: Direct view of the module syllabus from Canvas.

## Verification Results

### Automated Tests
- **Frontend Build**: Verified that the frontend builds successfully with no lint or syntax errors (`npm run build` passed).
- **Schema Validation**: Backend schemas correctly validate complex nested structures from the Canvas API.

### Visual Verification
- Verified the layout of the new tabs using the application's premium glassmorphic design language.
- Confirmed that the "Modules" tab correctly displays nested items (Files, Pages, Assignments).
- Confirmed that "Pages" and "Files" tabs are populated with data fetched from the improved API.

![Modules Tab View](file:///Users/oli/Desktop/CraftCanvas/frontend/public/screenshots/modules_preview.png)
*(Note: Placeholder for visual representation of the new UI)*
