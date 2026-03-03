# Expanding AI Document Access (RAG Extraction)

The AI currently supports PDF and TXT file extraction but fails to extract text from other common lecture formats like PowerPoint (PPTX), Word (DOCX), CSV, and HTML. This prevents the AI from "reading" a large portion of Canvas module materials.

## Proposed Changes

We will upgrade the extraction pipeline to support a wider array of document formats so they can be indexed into the AI's knowledge base.

### Backend

#### [MODIFY] `lib/extraction.py`(file:///Users/oli/Desktop/CraftCanvas/backend/lib/extraction.py)
- **Add PPTX Support**: We have installed `python-pptx` to parse `.pptx` presentations and extract text from all slides and shapes.
- **Add HTML Support**: We have installed `beautifulsoup4` to clean and extract plain text from `.html` files.
- **Add CSV Support**: Use the built-in `csv` module to parse tabular `.csv` files into readable text content.
- **Fix DOCX**: Ensure `python-docx` is correctly used for `.docx` files.

## Verification Plan

### Automated Re-Indexing
We will write and run a script to find all `canvas_files` that have `is_indexed = False` or where extraction previously failed silently, and force them through the new extraction pipeline.

### Manual Verification
Ask the AI to retrieve specific information only found within a newly indexed PPTX or CSV file to verify the RAG system is successfully searching through the new formats.
