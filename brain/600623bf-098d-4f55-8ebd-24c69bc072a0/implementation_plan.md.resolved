# Enhancing Canvas Data Access for AI

This implements the user's request to "make it such that AI can access and analyse as much of data for my modules from canvas as it can... and implement them as tools for the AI."

## User Review Required
> [!IMPORTANT]
> This plan adds significant new sync capabilities to the app, which might cause initial database migrations/creations. The new tables will be added automatically by SQLModel.

## Proposed Changes

### `backend/models`
- #### [MODIFY] [course.py](file:///Users/oli/Desktop/CraftCanvas/backend/models/course.py)
  - Add `syllabus_body: Optional[str] = None` to the `Course` model to store the raw HTML syllabus.
- #### [NEW] `backend/models/canvas_page.py`
  - Create a `CanvasPage` model to store wiki pages (canvas_id, course_id, title, url, body_text_extracted).
- #### [NEW] `backend/models/canvas_module.py`
  - Create `CanvasModule` and `CanvasModuleItem` models to store the structural outline of the course (e.g., Week 1 -> [Page A, File B]).

### `backend/database.py`
- #### [MODIFY] [database.py](file:///Users/oli/Desktop/CraftCanvas/backend/database.py)
  - Import `canvas_page` and `canvas_module` to ensure SQLModel creates the tables on startup.

### `backend/services/canvas_sync.py`
- #### [MODIFY] [canvas_sync.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/canvas_sync.py)
  - Update `sync_courses` to request the syllabus by adding `"include": ["term", "syllabus_body"]` to the params, and saving `syllabus_body` when creating/updating courses.
  - Add `sync_modules` to fetch `/api/v1/courses/{id}/modules` and their items (`/api/v1/courses/{id}/modules/{id}/items`), storing them in the new models.
  - Add `sync_pages` to fetch `/api/v1/courses/{id}/pages`, safely extracting their text if they are not already synced. These pages will also be ingested into the `rag_service` automatically so that the `search_module_materials` tool can search through them.
  - Call `sync_modules` and `sync_pages` in `sync_all`.

### `backend/services/ai_tools.py`
- #### [MODIFY] [ai_tools.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_tools.py)
  - **New Tool 1:** `get_course_syllabus(course_code)`: Retrieves the `syllabus_body` for a course, returning it as stripped markdown/text.
  - **New Tool 2:** `get_course_modules_outline(course_code)`: Retrieves the Week-by-Week (or Module) structural outline for the course, listing its items.
  - **New Tool 3:** `get_canvas_page_content(course_code, page_title)`: Retrieves the extracted text from a specific Canvas page by title.
  - Append these functions to the `TOOL_DEFINITIONS` list.

### `backend/services/context_assembler.py`
- #### [MODIFY] [context_assembler.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/context_assembler.py)
  - Update the `_action_instructions` to inform the AI about the three new tools natively.

## Verification Plan
### Automated Tests
- I will mock the API calls and add tests in `tests/test_services/test_canvas_sync.py` and `test_ai_tools_documents.py` to ensure the tools return the expected database data without throwing errors.

### Manual Verification
- Restart the backend to apply SQLModel migrations.
- Run a Canvas Sync to populate the database with real modules and syllabus data.
- The AI should successfully use these tools when asked "What's in Week 1 for CS2103T?" or "Show me the syllabus for my course."
