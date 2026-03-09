# Implementation Plan - New Music Visualizations for Focus Tab

The goal is to enhance the Focus tab with more premium and engaging music-related visualizations. Currently, the tab has some background animations and a vinyl player. We will add new visualization modes that feel premium and help with focus.

## Proposed Changes

### [Frontend] Focus Page Enhancements

We will introduce several new music-synced visualizations and fix the UI layout.

#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/focus/page.tsx)
- **UI Fix**: Adjust the main glass container to ensure it doesn't overlap with the header or settings.
- **Modal Fix**: Ensure the customise modal has proper backdrop-blur and doesn't feel disconnected.
- **Task List UI**: Fix the "Focusing On" card styling to be more consistent.

#### [MODIFY] [SpotifyPlayer.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/SpotifyPlayer.tsx)
- **Music Visualizers**: Implement selectable visualizer modes:
    - `Bars`: Animated bars reflecting "energy" levels.
    - `Waveform`: A flowing waveform around the vinyl center.
    - `Rings`: Concentric circles pulsing to the beat.
- **Toggle**: Add a button to switch between the Vinyl art and Visualizer modes.
- **Dynamic Backgrounds**: Allow backgrounds to "react" (subtle pulse) to music playback state.

## Verification Plan

### Manual Verification
1.  Navigate to the Focus tab.
2.  Open the Customise Modal (Gear icon).
3.  Test each new background preset:
    - **Cyber Pulse**: Verify the HUD ring is pulsing and looks premium.
    - **Lava Lamp**: Verify the blobs move smoothly and have a glassmorphic/organic feel.
    - **Nebula**: Verify the cosmic effect is subtle and not distracting.
    - **Bokeh**: Verify the light particles look high-quality.
4.  Play music via Spotify and verify if the Vinyl player looks enhanced with the new glow/pulse.
5.  Switch between different modes (Focus/Break) and ensure backgrounds handle the transition well (if applicable).

### Automated Tests
- Run existing E2E tests for the focus flow to ensure no regressions:
  ```bash
  cd frontend && npx playwright test e2e/focus_flow.spec.ts
  ```
