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

- **RAG Retrieval**: Verified that searching for specific topics (e.g., "Quantum Computing") returns the relevant document chunks.
- **Linking Metadata**: Confirmed that the AI tools return the necessary IDs to build functional download links.
- **AI Formatting**: Updated the system instruction set to ensure the AI uses these links in its responses.

> [!TIP]
> You can now ask the AI things like "Where can I find the slides for lecture 3?" or "Show me my GEX readings" and it will provide clickable links directly in the chat.
