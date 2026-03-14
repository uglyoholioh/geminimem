# Expansion Plan: Multimodal, Indexed & Interactive Video Context

This plan outlines the major expansion of the Lecture Video Analyser into an end-to-end "AI Memory" tool.

## Phase 1: Multimodal Visual Reasoning
Instead of relying solely on OCR, we will feed keyframe images directly into Gemini 1.5 Pro. This allows the AI to "see" diagrams, tables, and visual emphasis.

### Proposed Changes
- **[GeminiService]**: Update `synthesize_chapter` to accept list of image paths and use them in model generation.
- **[CompacterExtractor]**: Pass actual keyframe files to the Gemini synthesis loop.

## Phase 2: Searchable Vector Indexing
We will implement a local vector store to make the "Compacted Context" searchable.

### Proposed Changes
- **[NEW] [indexing_service.py]**: Integration with ChromaDB.
- **[processing/worker]**: Automatically index segments upon completion.

## Phase 4: Intelligent "Jump Points"
We will enhance the AI assistant to identify specific timestamps that answer the user's question or provide prerequisite context.

### Proposed Changes
- **[web/api/main.py]**: Update the `/api/chat` prompt to return a JSON structure containing the answer and a list of `jump_points` (timestamp + reason).
- **[web/static/app.js]**: Render jump points as interactive buttons in the chat bubble.

## Phase 5: Automated Metadata Enrichment
Extracting external links and local resources to create a more comprehensive knowledge index.

### Proposed Changes
- **[indexing_service.py]**: Add fields for external URLs and linked resource filenames.
- **[CompacterExtractor]**: Regex scan for URLs in transcript and OCR text.

## Phase 6: GPU-Accelerated OCR
Migrating to EasyOCR to leverage the user's RTX 4070 Super for faster frame analysis.

### Proposed Changes
- **[ocr_service.py]**: Implement `EasyOCRProvider` with CUDA detection.

## Verification Plan
1. **Jump Point Test**: Ask "What happens at the 5-minute mark?" and verify it provides a clickable link.
2. **Metadata Test**: Include a URL in a mock transcript and verify it appears in the segment metadata.
3. **OCR Performance**: Compare processing time of EasyOCR (GPU) vs Tesseract (CPU).
