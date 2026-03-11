# Expanding Companion Animations and Persistent UI

This plan outlines the steps to make the AI Companion persistent within the chat interface, expand its animation state machine, and map the user-provided sprite references to interactive moods/actions.

## User Review Required
> [!IMPORTANT]
> The companion currently disappears when you start chatting. I propose turning it into a **Persistent Dynamic Header** at the top of the chat view.
> As you chat, the AI will dictate the Companion's mood (e.g., if the AI is providing a warning, the Companion will look `surprised` or `angry`). 
> I also added a few more interactive animations (like `playing` with yarn when clicked) based on your references. Let me know if you are happy with this approach!

## Proposed Changes

### 1. `frontend/components/chat/DailyBriefChat.tsx`
#### [MODIFY] DailyBriefChat.tsx
- Restructure the UI: Move `<CompanionSprite>` out of the empty "Ask about your schedule" block and place it in a sticky header container at the top of the chat container.
- Update the `derivedMood` logic to analyze the latest assistant message. The AI can respond with implicit sentiment (e.g. if the response includes "great", "error", etc.) or we can parse specific mood tags that we instruct the AI to return. For now, we will simply guess mood based on keywords in the AI's response text and the context of the chat.
- Add interactions: clicking the persistent companion could trigger random active animations like `playing` or `waving` before returning to its chat-based mood.

### 2. `frontend/components/companion/CompanionSprite.tsx`
#### [MODIFY] CompanionSprite.tsx
- Expand `CompanionMood` to include `surprised`, `winking`, `angry`, `sad`, `confused`, `playing`, `waving`, `running`, `jumping`, `charging`, `glitch`.
- Add SVG logic for `soccat` eyes and mouth to match the reference images:
  - `surprised`: Hollow square eyes, small 'o' mouth.
  - `winking`: One line eye, one open eye.
  - `angry`: Angled `>` `<` pixel eyes.
  - `sad`: Droopy eyes, added tear drops.
  - `confused`: Eyes looking sideways, `?` above head.
- Override body rendering based on action:
  - `playing`: Paw stretched out to a yarn ball.
  - `waving`: One paw raised high.
  - `running`: Action lines, stretched posture.
  - `jumping`: Floating translation, shadow under sprite.
  - `charging`: Draw a wall socket and connect the tail to it.
  - `glitch`: Pixelated dissolution effect.
- Map transitions and CSS animations to the new moods.

## Verification Plan
1. Send various chat messages and see the Companion change expressions persistently during the chat.
2. Click the specific action buttons mapping to the new SVG facial expressions and body poses to verify they render correctly for `soccat`.
3. Ensure chat layout adjusts nicely with the new persistent header.
