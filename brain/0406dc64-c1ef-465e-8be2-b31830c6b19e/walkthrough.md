# Walkthrough: Intelligent Brief Correction & Prompt Recipes

I have implemented two significant refinements to the CraftCanvas "Command Center" to make it more intelligent and efficient.

## 1. Intelligent Brief Correction
Each section of the Daily Brief now has interactive feedback buttons (hidden until you hover over a section).

- **Positive Feedback**: Lets the AI know you like the content/style.
- **Negative Feedback**: Specific prompts allow you to tell the AI what to improve (e.g., "Stop mentioning CS2103T announcements" or "Keep it shorter").
- **Persistence**: These preferences are saved locally and injected into the AI system prompt every morning to personalize your next generation.

## 2. Custom Prompt Recipes
Above the chat input, you'll now find mono-styled "Prompt Recipes". These are curated shortcuts for high-value academic workflows.

- **[EXAM_PREP]**: Generates a study plan based on your materials and deadlines.
- **[SYLLABUS_CHECK]**: Summarizes learning objectives for all active modules.
- **[TASK_BREAKDOWN]**: Splits large assignments into manageable daily sub-tasks.
- **[WEEKLY_RECAP]**: Provides a high-level summary of your academic progress.

## Technical Summary
- **Backend**: New `POST /brief/feedback` endpoint and updated prompt logic in `brief_generator.py`.
- **Frontend**: New `PromptRecipes.tsx` component and interactive section handling in `DailyBriefSummary.tsx`.
