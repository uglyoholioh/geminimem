# Improve UI for Focus Page

## Goal
Improve the UI and layout of the Focus page. Specifically, address the timer text getting cut off on smaller screens/dense layouts, improve spacing, and polish the aesthetics.

## Proposed Changes

### `frontend/app/focus/page.tsx`
- **Main Container**: Increase max width (`max-w-[95vw] xl:max-w-[1400px]`) when Spotify is enabled to give all three columns adequate space.
- **Clock Renderers**: Replace static huge text classes (e.g., `text-8xl md:text-9xl`) with responsive `clamp()` functions for `fontSize`. This ensures the timer dynamically scales to fit the column width without getting clipped on the left.
- **Middle Column adjustments**: Make sure the clock container has full width and text centers itself, avoiding clipping. Add a subtle container for the mode selector.
- **Right Column (Tasks)**: Improve the visual hierarchy of the "FOCUSING ON" section and empty state styling for a more polished look.

## Verification Plan
1. Open the Focus page.
2. Confirm the timer text is no longer cut off.
3. Verify the layout adapts gracefully at different window sizes.
4. Ensure the aesthetics look premium and balanced.
