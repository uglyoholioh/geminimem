# Implementation Plan - Enhancing AI Retrieval and Linking for Module Materials

The user wants the AI to easily retrieve and link to module materials. Currently, RAG is disabled, and AI tool responses for materials don't include direct links.

## Proposed Changes

### Backend

#### [MODIFY] [rag_service.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/rag_service.py)
- Re-enable RAG service logic (or implement a lightweight keyword-based version if ChromaDB is problematic).
- Ensure `query` returns enough metadata to construct links (IDs, filenames, course codes).

#### [MODIFY] [ai_tools.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_tools.py)
- Update `search_module_materials` and `search_across_documents` to return structured results including file IDs and types.
- Ensure the result format is consistent for the LLM to process.

#### [MODIFY] [ai_service.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_service.py)
- Update system prompt instructions to encourage the LLM to use the materials tools and provide markdown links in the format `[filename](file:///api/v1/courses/{course_id}/files/{file_id}/download)`.
- *Note*: Even though the frontend uses these links, the AI should be instructed on the correct format.

### Frontend

#### [MODIFY] [BriefChat.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/brief/BriefChat.tsx) (Assuming this is the component)
- Ensure the chat renderer correctly handles file download links.
- Add a "Materials" tab or quick link section in the chat if appropriate.

## Verification Plan

### Automated Tests
- `pytest backend/tests/test_rag_retrieval.py` (if it exists or create one) to verify the RAG query returns expected files.
- Verify that `ai_service` correctly calls the tools and includes links in its response using a mocked LLM response if possible.

### Manual Verification
- Ask the user to query the AI: "Show me my slides for week 3 of GEX1015".
- Verify that the AI provides a response with a clickable link to the file.
- Verify that clicking the link downloads/opens the correct file.
