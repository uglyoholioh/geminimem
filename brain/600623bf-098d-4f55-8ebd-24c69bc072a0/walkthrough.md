# Walkthrough: Adding Media Retrieval and Subtitling to AI Tools

I have successfully added the two requested AI Tools that enhance the capabilities of the assistant when working with user documents.

## Changes Made

### 1. `get_media_link`
We introduced a tool that allows the assistant to quickly grab a formatted download or view link for any file (e.g. `slides.pdf` or `lecture.mp4`) previously synced from Canvas or uploaded manually to a module. 
- It uses the local search algorithm matching filename and course filters.
- Instructs the AI to give the user a clickable download element like: `[Download lecture.mp4](/api/v1/...)`.

### 2. `generate_video_subtitles`
We added a new processing action that:
- Locates the local downloaded video file.
- Checks if the user's Google GenAI API key is configured.
- Uploads the video securely to the Gemini File API.
- Polls for the `PROCESSING` state until the video is ready to be transcribed.
- Prompts the `gemini-2.5-pro` model to output a chronological transcript or subtitle (.srt) format natively from the video's audio.
- Trashes the file on Gemini's servers immediately after generation to ensure privacy.
- Returns the full generated transcript to the AI to present to the user.

### 3. Context & Instruction Updates
We updated `context_assembler.py` to mention both `get_media_link` and `generate_video_subtitles`. This is injected into the AI system prompt dynamically, so it now intrinsically knows *when* to seamlessly trigger these tasks based on user requests (e.g., "Give me a transcript for the week 3 video" or "Link me the syllabus pdf").

## Validation Results
We added mocks and test cases for both `get_media_link` and `generate_video_subtitles` to `test_ai_tools_documents.py`. All 26 backend test cases currently pass without regressions, ensuring that the tool schemas return properly and interact effectively with the `google.genai.Client`.

---

# Walkthrough: Enhancing Canvas Data Access for AI

In this follow-up section, I have implemented full support for the AI to fetch and analyse core Canvas structural materials including the course syllabus, week-by-week modules, and individual wiki pages.

## Changes Made

### 1. New SQLModels
- **`CanvasPage`**: Stores the raw HTML and extracted plaintext for Wiki pages on Canvas.
- **`CanvasModule` & `CanvasModuleItem`**: Stores the chronological outline of the course and references to files, pages, and assignments within those weeks.
- Added `syllabus_body` directly to the `Course` model.

### 2. Canvas Sync Updates
- Refactored `canvas_sync.py` to now fetch `syllabus_body` during the `/api/v1/courses` call.
- Added two new complex background parsers: `sync_modules` and `sync_pages`, which are called in the global `sync_all` block.
- Any fetched `CanvasPage` is now also natively embedded into the `rag_service` for general `search_module_materials` keyword lookups by the AI.

### 3. New AI Executive Tools
I have provided three new specific tools to the LLM agent via `ai_tools.py` and `context_assembler.py`:
- `get_course_syllabus`: Instructed as the go-to resource for logistics, grading criteria, and overview questions.
- `get_course_modules_outline`: Instructed to be used to answer questions about the course timeline (e.g. "What are we doing in Week 3?").
- `get_canvas_page_content`: Enables the AI to read specific wiki pages.

## Validation Results
- Python compiler and database initialization routines executed flawlessly.
- `pytest` for AI documents passed all 26 tests, protecting regression. The AI will now begin dynamically fetching and referencing dense course documents!
