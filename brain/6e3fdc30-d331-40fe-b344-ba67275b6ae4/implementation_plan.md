# Tamagotchi Companion Dashboard Widget

The goal is to redesign the AI companion to be a Tamagotchi-style dashboard widget replacing the standard `CompanionMini` component. It will live on the dashboard, feature an interactive design (egg or console shape, physical-looking buttons, LCD screen), and express behaviors/moods based on the user's interaction with the app.

## Proposed Changes

### `frontend/components/dashboard/widgets/CompanionMini.tsx`
[MODIFY] Rewrite the component to feature a Tamagotchi aesthetic.
- **Visuals**: Give the widget an egg/device shape, a recessed "screen" area containing the `CompanionSprite` and stats, and interactive "buttons" to trigger chat or interactions.
- **Behavior**: It will respond to app data (focus time, task completion) to set its mood and dialogue, showing that it reacts to the user's study habits.
- **Interactions**: Add a way to "feed" or "pet" the companion (perhaps a button that awards a tiny amount of EXP or just triggers a happy animation), or a button to open the chat interface within the widget.

### `frontend/components/companion/CompanionSprite.tsx`
[MODIFY] (If needed) Adjust the sprite animations or framing so it fits nicely inside the Tamagotchi screen aspect ratio.

## Verification Plan

### Automated Tests
- Check for build errors with `npm run build`.

### Manual Verification
- View the dashboard widget to ensure it closely resembles a digital pet device.
- Verify that interactions (clicking buttons) work and update the state or show dialogue.
