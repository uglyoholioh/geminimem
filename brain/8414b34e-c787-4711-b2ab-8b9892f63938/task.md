# AI Tooling Expansion

## Planning
- [x] Analyse current AI tools (`ai_tools.py`, `rag_service.py`, `ai_service.py`)
- [x] Analyse extraction pipeline (`extraction.py`)
- [x] Review context assembly and system prompt
- [x] Review frontend action cards and chat rendering
- [x] Write implementation plan
- [x] Get user approval

## Execution — Backend New Tools
- [x] Add `summarize_document` tool (AI-powered single doc summary)
- [x] Add `search_across_documents` tool (multi-course RAG search)
- [x] Add `get_document_outline` tool (headings/structure extraction)
- [x] Add `extract_key_concepts` tool (structured concept extraction)
- [x] Add `compare_documents` tool (cross-doc comparison)
- [x] Register all new tools in `TOOL_DEFINITIONS`

## Execution — RAG Improvements
- [x] Upgrade chunking to sentence-aware with overlap
- [x] Add filename metadata to RAG chunks for better citation
- [x] Improve `search_module_materials` to return richer metadata (page numbers, section headings)

## Execution — System Prompt Updates
- [x] Update `context_assembler.py` to document new tools for the AI
- [x] Add document-search-specific instructions

## Verification
- [x] Write unit tests for new tool functions
- [ ] Manual end-to-end test via chat
