# Canvas Data Processing Enhancement

Ensure all relevant Canvas data (PDFs, Pages, Syllabi, Announcements, and Assignments) are fully processed and searchable by the AI.

## Proposed Changes

### Database Models

#### [MODIFY] [announcement.py](file:///Users/oli/Desktop/CraftCanvas/backend/models/announcement.py)
- Add `is_indexed` field to track RAG status.

#### [MODIFY] [assignment.py](file:///Users/oli/Desktop/CraftCanvas/backend/models/assignment.py)
- Add `is_indexed` field to track RAG status.

#### [MODIFY] [course.py](file:///Users/oli/Desktop/CraftCanvas/backend/models/course.py)
- Add `is_indexed` field to track syllabus indexing status.

#### [MODIFY] [canvas_file.py](file:///Users/oli/Desktop/CraftCanvas/backend/models/canvas_file.py)
- Add `indexing_status` field to track failures (pending, success, failed, skipped).

### Sync Service

#### [MODIFY] [canvas_sync.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/canvas_sync.py)
- Implement background indexing tasks for Announcements, Assignments, and Course Syllabi.
- Update `sync_all` to queue these tasks.
- Improve `index_file_background` with better status tracking.

## Verification Plan

### Automated Tests
- Create a test script to verify that unindexed materials are correctly picked up and added to ChromaDB.
- Verify that `rag_service.query` returns results from announcements and assignments after processing.

### Manual Verification
- Ask the AI: "Search my announcements for anything about the midterm."
- Ask the AI: "What's in the syllabus for CS2103T?"
