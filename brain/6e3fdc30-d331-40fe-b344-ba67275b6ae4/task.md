# Tasks

- [x] Add `soccat` and other NUS mascots to `CompanionSprite.tsx`
- [x] Update `CommandCenter.tsx` to fetch companion state
- [x] Update `DailyBriefChat.tsx` to replace icons with the `CompanionSprite`
- [x] Remove `CompanionMini` component and references from dashboard registry
- [x] Verify UI builds and looks native to the chat

## Follow-up Iterations
- [x] Update `soccat` design to match the provided white cat with a plug tail illustration
- [x] Add click/pet interaction to the Companion sprite in the empty state
- [x] Make the Companion react to the user's input or chat messages

## Interactive Expansion
- [ ] Refactor `DailyBriefChat` layout so the Companion remains visible (persistent) during the chat.
- [ ] Expand `CompanionMood` in `CompanionSprite` to include: surprised, winking, angry, sad, confused, playing, waving, running.
- [ ] Implement SVG details for the new moods/animations for `soccat`.
- [ ] Add more interactive triggers (e.g. random click animations like playing yarn, or mood driven by chat sentiment/suggestions).
