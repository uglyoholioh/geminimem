# Expanding AI Tool Access & Canvas Data

The user wants the AI to have full access to Canvas data (materials, announcements, etc.) and more CRUD operations. 

## Proposed Changes

### [AI Tools Component]

#### [MODIFY] [ai_tools.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_tools.py)
- **Enhanced Tools**:
    - `search_announcements(...)`: Now returns the full `message_html` (stripped of HTML tags for the LLM) to allow the AI to actually read the content of announcements.
    - `get_courses()`: Include more summary data.
- **New Tools**:
    - `search_module_materials(query: str, course_code: Optional[str] = None)`: Uses the **RAG Service** to search inside the *contents* of uploaded files, Canvas documents, and synced materials. This is the "everything" access.
    - `get_module_details(course_code: str)`: Returns rich metadata from the `Course` model.
    - `create_note(...)`, `update_note(...)`, `delete_task(...)`: CRUD operations.
    - `create_timetable_event(...)`, `delete_timetable_event(...)`: Sanitized timetable tools.

- **Update `sync_all`**: Integrate `sync_files` and indexing into the main sync loop.

#### [MODIFY] [ai_tools.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_tools.py)
- **Enhanced Tools**:
    - `search_announcements(...)`: Already returning `message_text`.
    - `search_module_materials(...)`: Uses **RAG Service** to search inside the *contents* of downloaded Canvas files. (Verified working, but needs data).

## Verification Plan

### Automated Tests
- Run the `test_rag_search.py` script (to be created) to verify:
    1. Canvas files are synced and downloaded.
    2. File content is extracted (PDF/Docx) and chunks exist in `rag_chunks`.
    3. AI can answer questions using `search_module_materials`.

### Manual Verification
- Test in the Command Center UI: "What is the assessment criteria for CS2030?"
- Test: "Create a note for me about my MA1522 study plan."
