# Enhancing AI Chat Interactivity

This plan addresses making the AI more interactive by allowing it to ask for clarification using clickable option buttons, and improving the responsiveness of the loading state.

## Proposed Changes

### 1. Backend: System Prompt Instructions
#### [MODIFY] [brief.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/brief.py)
Update the `_build_brief_chat_context` system prompt to instruct the AI on how to output a new action block type: `ask_clarification`.
- The instructions will tell the AI that if a user query is too broad (e.g., "what's the deadline?" without specifying a course), it should output:
  ```json
  :::action
  {"type":"ask_clarification","question":"Which course?","options":["CS2030", "BT1101"]}
  :::
  ```

### 2. Frontend: Action Parsing
#### [MODIFY] [parseActions.ts](file:///Users/oli/Desktop/CraftCanvas/frontend/lib/parseActions.ts)
- Extend the `ActionData` interface to include the new type `ask_clarification`, along with `question?` (string) and `options?` (string array) properties.

### 3. Frontend: Action Rendering
#### [MODIFY] [ActionCard.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/chat/ActionCard.tsx)
- Add rendering logic for the `ask_clarification` type.
- Instead of the standard action rendering (which looks like a task card with an "Add/Done" button), this will render the specific clarification `question` and display the `options` as a horizontal or wrapped list of clickable buttons.
- Update the component signature to optionally accept an `onSend(text: string)` callback from the parent, so clicking an option automatically sends it as a user message.

### 4. Frontend: Chat Component & Loading State
#### [MODIFY] [DailyBriefChat.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/chat/DailyBriefChat.tsx)
- Pass the `onSend` callback down to the `ActionCard` components being rendered within assistant messages.
- Revamp the loading indicator: Enhance the visual feedback when waiting for the AI response. Since the AI often takes 10-15 seconds to search RAG and use tools before streaming starts, we will update the typing indicator to show text like **"Thinking & searching materials..."** alongside a pulsing or spinning icon to reassure the user that work is happening in the background.

## Verification Plan

### Manual Verification
1. I will ask a vague question like "What's the deadline?"
2. Check if the AI outputs the `ask_clarification` action block with course options.
3. Verify the frontend renders this action as clickable buttons.
4. Click an option and ensure it fires off a new user message.
5. Verify the new loading indicator is visible and reassuring immediately after sending a message.
