# Material Manipulation AI Tools

This plan implements three new advanced tools allowing the AI to manipulate and convert course materials into active productivity formats.

## Proposed Changes

### 1. `breakdown_assignment_into_tasks`
- **Goal:** Convert massive Canvas assignments into granular, actionable sub-tasks.
- **Logic:** 
  - Takes `assignment_id` (which the AI can find using `search_tasks`).
  - Retrieves the `Assignment` from the DB to get the `description_html` and `due_at`.
  - Cleans the HTML and sends it to Gemini: "Break this assignment down into 3-5 chronological sub-tasks to complete before the deadline. Output valid JSON: `[{"title": "Read chapter 4", "days_before_due": 5}]`."
  - Parses the JSON, calculates the exact `due_date` for each task based on the assignment deadline, and inserts them into the `Task` table with the `assignment_id`.
  - Returns a success message listing the created tasks.

### 2. `generate_flashcards_export`
- **Goal:** Convert course materials into an importable CSV for Anki or Quizlet.
- **Logic:**
  - Takes a `course_code` and `topic_or_week`.
  - Uses the same RAG lookup mechanism as the quiz generator.
  - Prompts Gemini: "Create 10-15 high-quality flashcards based ONLY on this text. Output JSON: `[{"front": "concept", "back": "definition"}]`."
  - The Python tool formats this JSON strictly into a CSV string.
  - Returns the CSV wrapped in a Markdown code block ` ```csv ... ``` ` so the user can easily copy and paste it into their spaced-repetition app.

### 3. `generate_mindmap`
- **Goal:** Visually map out course concepts.
- **Logic:**
  - Takes a `course_code` and `topic_or_week`.
  - Pulls RAG context.
  - Prompts Gemini: "Create a Mermaid.js mindmap diagram connecting the key concepts of this material. Output valid mermaid code."
  - Returns the Mermaid block to the Chat UI, which natively supports rendering Mermaid graphs.

### 4. `context_assembler.py` Updates
- Expose all three new tools in the AI system instructions so the assistant natively offers "I can break this assignment down for you" or "Shall I generate a mindmap / flashcards?".

## Verification Plan
### Automated Tests
- Mock Gemini responses for all three tools in `test_ai_tools_documents.py`.
- Ensure `breakdown_assignment_into_tasks` correctly calculates dates and adds rows to the database.

### Manual Verification
- Ask the AI to "Break down my upcoming essay".
- Ask the AI to "Generate a mindmap for CS2103T Software Engineering Principles".
- Ask the AI to "Make Anki flashcards for Week 3".
