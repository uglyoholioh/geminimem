# Focus Tab Redesign

This plan outlines the changes to make the Focus tab more "minimalistic glassmorphic" while fleshing out features. 

## User Review Required

> [!WARNING]
> The `DESIGN_SYSTEM.md` specifies a "dense terminal" aesthetic with no gradients, no shadows, and no rounded-everything. A "glassmorphic" aesthetic usually relies on blurred transparent backgrounds, shadows, and rounded corners.
> 
> **My proposed middle-ground:** We will use sharp, dark glassmorphism. This means dark, semi-transparent backgrounds with background-blur (`bg-surface/60 backdrop-blur-xl`), strict 1px borders (`border-border`), and keeping the typography/colors strictly within the design system (amber accents, DM Mono data). It will feel premium and glassmorphic but maintain the terminal vibe. Let me know if you approve this direction or want me to lean fully into standard glassmorphism (rounded, lighter)!

## Proposed Changes

### Frontend - Focus UI

#### [MODIFY] [FocusPage](file:///Users/oli/Desktop/CraftCanvas/frontend/app/focus/page.tsx)
- Redesign layout to feel more open and minimal. The central timer will be more prominent.
- Implement the refined dark-glass styling over the backdrops.
- Integrate an **Ambient Sounds player** (Rain, Cafe, White Noise) with volume controls directly on the Focus page.
- Add an in-place checkbox to **mark the active task as completed** without leaving the Focus page.
- Display a small **Session Stats** pill (e.g., "🍅 3 completed today") tracking local session progress.

#### [MODIFY] [FocusTimerProvider](file:///Users/oli/Desktop/CraftCanvas/frontend/components/FocusTimerProvider.tsx)
- Fetch and persist custom Pomodoro durations (`focus_duration`, `short_break_duration`, `long_break_duration`) from the backend settings API.
- Allow updating these durations when the user saves them in the "Customise Appearance" modal.
- Add local state for total Pomodoros completed in the current session.

### Backend

- *No backend changes needed.* The `/settings` router already supports arbitrary key/value upserts (via `/settings/{key}`), which cleanly handles saving custom Pomodoro times. Task completion already uses the existing `PUT /tasks/{id}` endpoint.

## Verification Plan

### Manual Verification
1. Start the frontend development server.
2. Navigate to the Focus tab.
3. Open the "Customise" modal and change the Focus, Short Break, and Long Break durations. Verify they apply to the timer.
4. Toggle ambient sounds and verify audio plays/pauses.
5. Select a task, start a timer, then mark the task as complete. Verify it is marked completed across the app.
6. Observe the visual "dark glass" design changes for aesthetic feel.
