# Task: Implement Brief Correction & Prompt Recipes

## Planning & Design
- [x] Analyze brief generation and chat implementation <!-- id: 100 -->
- [x] Design feedback storage mechanism (using local SQLite via Settings) <!-- id: 101 -->
- [x] Design prompt recipe UI component (mono-styled horizontal scroll) <!-- id: 102 -->

## Backend: Brief Correction
- [x] Create API endpoint `POST /brief/feedback` <!-- id: 104 -->
- [x] Update `_build_prompt` in `brief_generator.py` to use `brief_feedback_*` settings <!-- id: 105 -->
- [x] Add logic to fetch feedback in `generate_brief` <!-- id: 103 -->

## Frontend: Brief Correction & Recipes
- [x] Implement feedback UI (thumbs up/down) in the Dashboard/Brief page <!-- id: 106 -->
- [x] Create `PromptRecipes` component with mono-styled buttons <!-- id: 107 -->
- [x] Integrate `PromptRecipes` into the chat input area <!-- id: 108 -->

## Verification
- [x] Test brief feedback end-to-end <!-- id: 109 -->
- [x] Verify prompt recipes trigger correct chat messages <!-- id: 110 -->
- [x] Create walkthrough of the new features <!-- id: 111 -->
