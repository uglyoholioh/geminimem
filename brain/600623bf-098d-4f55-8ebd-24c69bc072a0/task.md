# Adding AI Tools

- [x] Analyze how tools are currently exposed to the AI (`context_assembler.py` and `ai_tools.py`)
- [x] Implement tool for retrieving specific videos/pdfs/materials and linking it for the user
- [x] Implement tool for taking a video and subtitling it
- [x] Ensure AI has permissions and tools injected correctly
- [x] Test the new tools

# Enhancing Canvas Data Access

- [x] Add `syllabus_body` to `Course` model
- [x] Create `CanvasPage` and `CanvasModule` models
- [x] Update `canvas_sync.py` to fetch syllabus, pages, and modules
- [x] Create AI tools for syllabus, modules outline, and page content
- [x] Update AI context instructions
- [x] Test the new tools internally

# Advanced Interactive AI Tools

- [x] Implement `find_available_slots` AI tool (Timetable/Events querying)
- [x] Implement `generate_practice_quiz` AI tool (RAG + Gemini processing)
- [x] Add both tools to `context_assembler.py` instructions
# Material Manipulation Tools

- [x] Implement `breakdown_assignment_into_tasks` parsing HTML to Tasks
- [x] Implement `generate_flashcards_export` for CSV Anki exports
- [x] Implement `generate_mindmap` using Mermaid.js
- [x] Update `context_assembler.py` and `ai_tools.py`
- [x] Add backend tests for all three functions
