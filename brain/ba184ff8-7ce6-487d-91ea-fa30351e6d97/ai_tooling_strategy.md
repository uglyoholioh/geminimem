# Strategic Plan: AI Tooling Expansion for Precision & Accuracy

To move from an assistant that "searches" to one that "understands and executes," we need to shift from generic RAG-heavy retrieval to atomic, specialized tools. This plan outlines 4 key areas of expansion.

## 1. Atomic Knowledge Exploration
Currently, AI relies on `search_module_materials` (RAG) for almost everything. RAG can hallucinate or miss specific files if the query doesn't match the embedding perfectly.

### Proposed New Tools:
- **`list_course_folders`**: Returns the actual folder structure of a course (e.g., "Assignments/Drafts", "Lectures/Week1"). This allows the AI to "browse" instead of just guessing.
- **`get_file_metadata`**: Retrieves specific metadata (size, last modified, exact filename) without reading the content. Great for "find the largest PDF" or "what was uploaded yesterday".
- **`verify_file_exists`**: A lightweight check that confirms if a file is locally synced or only on Canvas.

## 2. Cross-Source Intelligence
The AI often struggles to link an announcement to a file or an assignment.

### Proposed New Tools:
- **`find_related_materials`**: Given an assignment, find the files or pages linked in its description.
- **`get_announcement_attachments`**: Specifically extracts and links files mentioned in a course update.
- **`link_task_to_material`**: Allows the AI to formally link a manual `Task` to a specific `CanvasFile` for one-click access in the UI.

## 3. Advanced Analysis Tools
AI currently "summarizes" by reading the first 4000 characters. For long research papers or large syllabi, this isn't enough.

### Proposed New Tools:
- **`query_document_deep`**: A specialized RAG tool that only searches *inside* a single file. This prevents cross-document "bleeding" and ensures 100% accuracy for that specific file.
- **`extract_table_data`**: Specialized parser for syllabus grading tables or schedule tables in PDFs, which generic text extraction often mangles.

## 4. Administrative & Self-Correction Tools
The AI needs to know when its data is stale.

### Proposed New Tools:
- **`check_sync_health`**: Let the AI check when the last Canvas sync was. If it's more than 24h old, the AI can suggest a manual refresh.
- **`report_broken_link`**: If the AI tries to download a file and it fails, it should have a tool to "mark" that file as missing, triggering a specialized re-sync background job.

## Implementation Roadmap

| Phase | Focus | Key Benefit |
|-------|-------|-------------|
| **Phase 1** | atomic verification | Zero "File not found" errors; perfect course matching. |
| **Phase 2** | deep document analysis | Accurate grading and syllabus breakdown. |
| **Phase 3** | autonomous sync management | Self-healing data pipeline. |

---

> [!TIP]
> **Tooling over Reasoning**: Every time we create a tool, we reduce the chance of the LLM "guessing" an answer. The goal is to make the AI an orchestrator of reliable tools.
