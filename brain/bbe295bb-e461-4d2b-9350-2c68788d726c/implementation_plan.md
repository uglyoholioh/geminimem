# Expanding AI Tool Access

The user wants the AI to have deeper access to module data (learning objectives, assessment criteria) and more CRUD operations. 

## Proposed Changes

### [AI Tools Component]

#### [MODIFY] [ai_tools.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_tools.py)
- **New Tools**:
    - `get_module_details(course_code: str)`: Returns rich metadata from the `Course` model (description, credits, workload, etc.).
    - `create_note(title: str, content: str, course_code: Optional[str] = None)`: Allow AI to create new notes.
    - `update_note(note_id: int, title: Optional[str] = None, content: Optional[str] = None)`: Allow AI to update existing notes.
    - `delete_task(task_id: int)`: Allow AI to remove tasks.
    - `create_timetable_event(...)`: Re-enable this tool with the correct sanitized signature (handling `**kwargs`).
    - `delete_timetable_event(...)`: Re-enable this tool with the correct sanitized signature.
- **Improved Tools**:
    - Update `get_courses` to include more summary data if requested.

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
