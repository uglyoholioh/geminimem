# Redesign AI Chat Experience on Dashboard

The current AI chat ("Command Center") is functional but feels utilitarian — cryptic `[LABEL]` prompt tags, plain text input, basic message bubbles, and a static "Thinking..." loading state. This plan redesigns it into a polished, modern AI chat experience that feels premium and inviting.

## Proposed Changes

### Chat Animations & Utilities

#### [MODIFY] [globals.css](file:///Users/oli/Desktop/CraftCanvas/frontend/app/globals.css)

Add new CSS keyframes and utility classes:
- **Typing indicator** — three bouncing dots animation (`@keyframes chat-bounce`)
- **Message entry** — subtle slide-up + fade-in for new messages (`@keyframes chat-message-in`)
- **Auto-growing textarea** — ensure the chat textarea resizes smoothly

---

### Prompt Recipes Redesign

#### [MODIFY] [PromptRecipes.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/chat/PromptRecipes.tsx)

Transform the cryptic `[EXAM_PREP]`, `[SYLLABUS_CHECK]` tags into human-readable, icon-enriched chips:

| Before | After |
|--------|-------|
| `[EXAM_PREP]` | 📝 Exam Prep |
| `[SYLLABUS_CHECK]` | 📋 Syllabus |
| `[TASK_BREAKDOWN]` | ✂️ Break Down Tasks |
| `[WEEKLY_RECAP]` | 📊 Weekly Recap |

Better pill styling with rounded, opaque backgrounds and subtle hover effects.

---

### Main Chat Component Overhaul

#### [MODIFY] [DailyBriefChat.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/chat/DailyBriefChat.tsx)

This is the largest change. Key improvements:

1. **Empty State Redesign** — Replace the plain "Suggested Prompts" header with a warm, personality-driven welcome:
   - AI avatar icon with greeting text ("How can I help you today?")
   - Suggestion cards as full-width buttons with descriptive text and an arrow icon, plus a subtle accent border
   - Each suggestion has an emoji prefix for scanability

2. **Message Bubbles** — Refine the visual treatment:
   - User messages: accent background with white text (keep) but add softer border-radius and max-width consistency
   - AI messages: clean `bg-surface` with a left accent stripe, the Zap icon retained as the AI avatar
   - Apply the `chat-message-in` entry animation to each message

3. **Typing Indicator** — Replace plain "Thinking..." with an animated three-dot bounce inside a styled bubble

4. **Input Area Redesign**:
   - Replace `<input>` with `<textarea>` that auto-grows up to 4 lines, shrinks back to 1 when empty
   - Replace the confusing rotated `Plus` send icon with `ArrowUp` (standard for modern chat UIs)
   - Wrap in a styled container with an inner border and subtle background

5. **PromptRecipes integration** — Keep the recipes row above the input but with the new human-readable design

---

### Dashboard Chat Section Refinement

#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/page.tsx)

- Refine the chat panel area with slightly better spacing between the Brief summary and the chat
- Ensure the chat area fills available space properly

---

### Fullscreen Brief Chat Refinement

#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/brief/page.tsx)

- Apply same layout refinements for consistency

## Verification Plan

### Browser Verification
1. Start the dev server: `cd /Users/oli/Desktop/CraftCanvas/frontend && npm run dev`
2. Open `http://localhost:3000` in the browser
3. Verify on the **dashboard page**:
   - Empty chat state shows the redesigned welcome with suggestion cards
   - Clicking a suggestion sends it and the typing indicator renders with animated dots
   - AI response appears with slide-in animation and clean styling
   - The textarea input auto-grows on multiline text and shrinks when cleared
   - Send button shows `ArrowUp` icon
   - PromptRecipes show human-readable labels with emojis
4. Navigate to `/brief` and verify the same chat improvements render correctly in fullscreen mode
