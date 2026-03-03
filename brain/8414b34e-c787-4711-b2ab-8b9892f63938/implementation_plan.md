# AI Document Tooling Expansion

Expand the AI's ability to search, summarise, and extract structured information from module documents (PDFs, slides, DOCX, etc.). Currently the AI has a single `search_module_materials` RAG tool with basic 500-char naive chunking. This upgrade adds 5 new tools and improves the RAG pipeline.

## Proposed Changes

### Backend — New AI Tools

#### [MODIFY] [ai_tools.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_tools.py)

Add **5 new tool functions** and register them in `TOOL_DEFINITIONS`:

| Tool | Purpose | How it works |
|------|---------|-------------|
| `summarize_document` | Summarise a single document by filename | Looks up the file in `CanvasFile`/`ModuleFile`, retrieves its `extracted_text`, and returns a truncated version (~4000 chars) for the LLM to summarise inline |
| `search_across_documents` | Search all modules at once (no course filter) | Calls `rag_service.query()` without `course_id`, returns top-10 results with source filenames |
| `get_document_outline` | Extract headings/structure from a document | Parses `extracted_text` for markdown-style headings, numbered sections, or slide boundaries |
| `extract_key_concepts` | Pull key terms/definitions from a document | Returns the first ~3000 chars of extracted text with metadata, letting the LLM identify concepts |
| `compare_documents` | Compare content across two documents | Fetches extracted text for two files and returns both (truncated) for the LLM to compare |

---

### Backend — RAG Improvements

#### [MODIFY] [rag_service.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/rag_service.py)

- **Sentence-aware chunking with overlap**: Replace naive `text[i:i+chunk_size]` with a paragraph/sentence-boundary-aware splitter that uses ~800 char chunks with ~100 char overlap. This prevents cutting mid-sentence and improves retrieval quality.
- **Richer metadata**: Store `filename`, `page_number` (where available), and `section_heading` in Chroma metadata so results can cite their source precisely.

#### [MODIFY] [search_module_materials in ai_tools.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_tools.py#L265-L327)

- Return `page_number` and `section` fields from metadata when available.
- Increase default `top_k` from 8 → 10 for broader coverage.

---

### Backend — System Prompt Updates

#### [MODIFY] [context_assembler.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/context_assembler.py)

Add a new section to the system prompt documenting the document-search tools, so the AI knows when to use each:

```
## Document Intelligence Tools
- search_module_materials: Search INSIDE document content (RAG). Use for specific factual questions.
- summarize_document: Get a full summary of a specific file. Use when the user says "summarise" or "what's in this file".
- search_across_documents: Search all courses at once. Use when the user asks a general academic question.
- get_document_outline: Get the structure/headings of a document. Use when asked "what topics does this cover".
- extract_key_concepts: Extract key terms and definitions. Use for revision/study prep.
- compare_documents: Compare two files side by side. Use when asked to compare or contrast materials.
```

---

### Backend — Extraction Enhancement

#### [MODIFY] [extraction.py](file:///Users/oli/Desktop/CraftCanvas/backend/lib/extraction.py)

- Add `extract_outline(filepath, content_type)` function that returns a list of heading strings extracted from each document type (PDF heading detection via font-size heuristics, PPTX slide titles, DOCX heading styles, etc.).

---

## Verification Plan

### Unit Tests

#### [NEW] [test_ai_tools_documents.py](file:///Users/oli/Desktop/CraftCanvas/backend/tests/test_services/test_ai_tools_documents.py)

Test the new tool functions with mocked DB sessions:
- `test_summarize_document_found` — mock a `CanvasFile` with `extracted_text`, verify truncated text returned
- `test_summarize_document_not_found` — verify error dict returned
- `test_search_across_documents` — mock `rag_service.query`, verify it's called without `course_id`
- `test_get_document_outline` — provide sample text with headings, verify parsed outline
- `test_compare_documents` — mock two files, verify both texts returned
- `test_smart_chunking` — verify new chunking respects sentence boundaries

Run: `cd /Users/oli/Desktop/CraftCanvas/backend && python -m pytest tests/test_services/test_ai_tools_documents.py -v`

### Manual Verification

> [!IMPORTANT]
> Please test by chatting with the AI after the changes are deployed. Suggested prompts:
> 1. *"Summarise the Week 3 lecture slides for CS2030"* — should trigger `summarize_document`
> 2. *"What topics are covered in the CS2040 tutorial PDF?"* — should trigger `get_document_outline`
> 3. *"Search all my modules for anything about recursion"* — should trigger `search_across_documents`
> 4. *"What are the key concepts in the Week 5 notes?"* — should trigger `extract_key_concepts`
> 5. *"Compare the Week 1 and Week 2 slides for CS2030"* — should trigger `compare_documents`
