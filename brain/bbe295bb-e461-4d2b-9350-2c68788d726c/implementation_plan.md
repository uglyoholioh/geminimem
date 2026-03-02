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

#### [MODIFY] [ai_service.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_service.py)
- Update `_call_gemini` and `_stream_gemini` tool injection logic to ensure `session` and `user_id` are consistently handled for the new tools. (Wait, I already did this with `**kwargs` inspection, but I should verify).

## Verification Plan

### Automated Tests
- Run the `test_chat_tools.py` script (to be created) to verify that the AI can now:
    1. Fetch module description/details for "CS2030".
    2. Create a new note about a module's assessment criteria.
    3. Create and then delete a timetable event.

### Manual Verification
- Test in the Command Center UI: "What is the assessment criteria for CS2030?"
- Test: "Create a note for me about my MA1522 study plan."
