# 🧶 Expanded Craft Integration: Detailed Implementation Plan

This plan outlines the technical steps to implement four advanced integration features between AcademicOS and Craft.

## Proposed Changes

### 1. 🔄 Bi-Directional Task Inbox
**Goal**: Automatically import tasks written in a specific Craft document.

- **[NEW] `backend/services/craft_inbox_sync.py`**
    - Implement `sync_craft_inbox()` which:
        - Fetches blocks from the configured "Inbox" Craft document using `GET /blocks`.
        - Identifies new blocks (comparing against a list of already-processed block IDs).
        - Passes block text to `task_parser.parse_task_nl`.
        - Creates `Task` records in the database.
        - (Optional) Appends a "Synced ✅" tag or moves the block in Craft if API allows (otherwise just tracks IDs).
- **[MODIFY] `backend/services/craft_sync.py`**
    - Add helper to fetch blocks from a specific page.
- **[MODIFY] `backend/database.py`**
    - Add `processed_craft_blocks` table to track synced block IDs and prevent duplicates.

### 2. 📁 Module Workspaces (Auto-Provisioning)
**Goal**: Create structured pages in Craft for every synced Canvas course.

- **[MODIFY] `backend/services/canvas_sync.py`**
    - After course upsert, trigger Workspace provisioning.
- **[NEW] `backend/services/craft_workspace_builder.py`**
    - Implement `ensure_course_workspace(course_id)`:
        - Check `CraftDocMap` if a workspace exists.
        - If not, create a "Parent" page for the course using `POST /blocks`.
        - Create sub-pages: `Lecture Notes`, `Deadlines`, `Resources`.
        - Push initial content (e.g., direct link to Canvas course).
- **[MODIFY] `backend/models/craft_doc_map.py`**
    - Ensure it stores `workspace_id`, `notes_page_id`, etc., for each course.

### 3. 📝 Automated Class Notes
**Goal**: Prepare a notes template in Craft before every class.

- **[NEW] `backend/jobs/class_notes_preparer.py`**
    - Scheduled job (e.g., run every hour).
    - Checks `TimetableSlot` for classes starting in the next 2 hours.
    - Creates a new block/page in the relevant Course's `Lecture Notes` section.
    - Template: `# [Module Code] Lecture - [Date]` + "Key Takeaways" + "Questions".

### 4. 📊 Central Task/Assignment Sync View
**Goal**: A single Craft document that always shows your current "To-Do" list from AcademicOS.

- **[NEW] `backend/services/craft_todo_sync.py`**
    - Implement `refresh_craft_todo_view()`:
        - Clears existing blocks in the "AcademicOS Dashboard" Craft document.
        - Pushes a fresh markdown list of:
            - Overdue tasks/assignments.
            - Due today.
            - High priority items.
    - Trigger this refresh whenever a task is updated or a sync finishes.

## User Review Required

> [!IMPORTANT]
> **Craft API Limitations**: The Craft API primarily supports *appending* or *adding* blocks. "Clearing and refreshing" a page (Plan 4) requires deleting specific blocks and adding new ones, which can be API-intensive. I will implement this with basic append-only "Weekly Status" updates if full refresh is too brittle.

> [!NOTE]
> **API Key Permissions**: These features require the AcademicOS Connection in Craft to have access to the specific folders where you want these pages created.

## Verification Plan

### Automated Tests
- `pytest backend/tests/test_services/test_craft_inbox.py`: Mock Craft API responses to verify task parsing and duplicate prevention.
- `pytest backend/tests/test_services/test_craft_workspace.py`: Verify correct folder/page hierarchy is generated.

### Manual Verification
1. **Inbox Sync**: Add a line "Finish CS2103T report by Sunday" in the Craft Inbox doc and verify it appears in AcademicOS tasks.
2. **Workspace Provisioning**: Trigger a Canvas sync and verify a new folder/page structure appears in your Craft Space.
3. **To-Do View**: Complete a task in AcademicOS and verify the Craft Dashboard doc updates (on next sync).
