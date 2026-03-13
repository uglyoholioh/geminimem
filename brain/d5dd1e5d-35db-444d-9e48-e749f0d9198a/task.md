# Enhancing Canvas Data Processing

Ensure the AI can retrieve and process Canvas data, including PDFs and other course materials.

## [ ] Research & Analysis
- [/] Review `canvas_sync.py` for file handling logic
- [ ] Review `rag_service.py` for PDF/content indexing logic
- [ ] Identify gaps in processing "other" materials (pages, modules, files)

## [x] Implementation
- [x] Update DB models with `is_indexed` and `indexing_status`
- [x] Implement background indexing for announcements/assignments/syllabi in `canvas_sync.py`
- [x] Improve `index_file_background` with better status tracking
- [x] Update RAG indexing to include all relevant course data

## [/] Verification
- [/] Test indexing logic for all entities (files, anns, assigns, syllabi)
- [ ] Verify AI can search and retrieve data from multi-source RAG
- [ ] Create walkthrough

## [ ] Verification
- [ ] Test AI's ability to answer questions from a Canvas PDF
- [ ] Test AI's ability to summarize a Canvas page
- [ ] Create walkthrough
