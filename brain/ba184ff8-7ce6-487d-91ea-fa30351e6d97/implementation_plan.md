# PROPOSAL: System Reliability & Intelligence Improvements

Following the implementation of the AI Feedback mechanism, I've identified several critical areas for improvement to ensure system stability and enhance AI utility.

## User Review Required

> [!WARNING]
> **Database Migration**: The `courses` table is missing the `syllabus_body` column, which is crashing background syncs. I will perform a manual migration (ALTER TABLE) to fix this.

## Proposed Changes

### 1. Sync Reliability (Backend)
- **Database Migration**: Run a SQL script to add `syllabus_body` to the `courses` table.
- **Error Handling**: Update `canvas_sync.py` to better handle missing tokens and provide actionable error messages in `sync_logs`.

### 2. AI Feedback: Re-categorization (Backend & Frontend)
- **[MODIFY] [ai_feedback.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/ai_feedback.py)**: Add logic to handle `re_categorize` action.
- **Task Conversion**: When an assignment is re-categorized as a task, the system will:
    1. Create a corresponding `Task` object.
    2. Add an `AIFeedback` record with `action='ignore'` for the original assignment.
- **[MODIFY] [AIFeedbackModal.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/feedback/AIFeedbackModal.tsx)**: Enable the "Re-categorize" button and add a target type selector (Task/Event).

### 3. Sync Health Indicator (Frontend)
- **[NEW] SyncStatusIndicator.tsx**: A small component for the Sidebar or Dashboard showing the status of the last Canvas sync.
- **Integration**: Show a "Warning" icon if the last sync failed, with a tooltip explaining the error (e.g., "API Token Missing").

### 4. AI Material Linking & Hallucination Fixes (Backend)
- **ID Ambiguity (Backend)**: Update `module_files.py` to handle both database `id` and `canvas_id` in the download path, or standardized on database `id` while ensuring all tools provide it.
- **Strict Course Filtering (Backend)**: In `ai_tools.py`, if a `course_code` is provided but no course is found, **return an error** instead of falling back to a global search. This prevents "CS2030S" hallucinations from finding "CS2030" materials.
- **Improved Course Matching**: Use exact matching (case-insensitive) for course codes to avoid prefix confusion.
- **Tool Output Standardisation**: Audit all tools (`search_module_materials`, `summarize_document`, etc.) to ensure both `course_id` and `file_id` are the internal database IDs, and clearly label them as such for the AI.
- **System Prompt Reinforcement**: Update `ContextAssembler` to warn the AI about the difference between internal IDs and Canvas IDs.

## Verification Plan

### Automated Tests
- **Migration Check**: Verify `syllabus_body` exists in SQLite after migration.
- **Logic Check**: Verify that re-categorizing an assignment creates a task and hides the source.

### Manual Verification
1. Trigger a manual sync from Settings and verify it succeeds without `OperationalError`.
2. Use the feedback button to re-categorize a "fake" assignment as a "Task" and verify it appears in the Task list.
