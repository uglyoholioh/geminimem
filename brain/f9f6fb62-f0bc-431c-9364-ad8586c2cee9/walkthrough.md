# Walkthrough - AI Material Retrieval and Linking

I have enhanced the AI's ability to retrieve and link to module materials.

## Changes Made

### 1. Re-enabled RAG Service
Updated [rag_service.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/rag_service.py) to use ChromaDB for document indexing and retrieval.
- Implemented `add_document` to chunk and embed module files.
- Implemented `query` to perform vector search for relevant materials.

### 2. Enhanced AI Tools
Updated [ai_tools.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_tools.py) to include `file_id` and `course_id` in search results.
- This allows the AI to construct direct download links.

### 3. Updated AI Instructions
Modified system prompts in [brief.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/brief.py) and [context_assembler.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/context_assembler.py).
- The AI is now instructed to provide download links in the format: `[filename](file:///api/v1/courses/{course_id}/files/{file_id}/download)`.

## Verification Results

- **RAG Retrieval**: Verified that searching for specific topics returns relevant document chunks with metadata.
- **Linking Metadata**: AI tools successfully return `file_id` and `course_id`.
- **UI Link Rendering Fix**: 
    - Switched from `file:///` to relative `/api/v1/` URIs to avoid browser security blocks.
    - Added a custom `ReactMarkdown` component in `DailyBriefChat.tsx` that transforms download links into premium styled buttons.
    - **Dual Options**: If a Canvas web URL is available, both "Download" and "View on Canvas" buttons are displayed side-by-side.

### 4. Canvas Page Linking
- **Data Model**: Added `canvas_web_url` to the `CanvasFile` model.
- **Synchronization**: Updated `canvas_sync.py` to link module item `html_url` back to the corresponding `CanvasFile`.
- **Retrieval**: Enhanced AI tools and RAG metadata to return Canvas web URLs alongside download links.

> [!TIP]
> Material links now appear as sleek, button-like cards with a "Download" icon, making them much easier to see and interact with!
