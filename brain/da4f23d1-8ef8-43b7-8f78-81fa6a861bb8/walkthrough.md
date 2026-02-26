# Focus Tab — Full Feature Walkthrough

## Bug Fixes
- **500 API Error**: Fixed missing `actual_minutes` column on `tasks` table and `user_type` column on `users` table by manually altering the SQLite schema.

## Features

### Pomodoro Timer
Focus (25m), Short Break (5m), Long Break (15m) modes with task time tracking via `actual_minutes`.

### Floating Timer Widget
When a timer is running and you navigate away, a floating pill in the bottom-right shows the live countdown with play/pause controls. Click it to jump back to Focus.

### Background Customisation
Click **✨ Customise** (top right) to open a tabbed modal:

**Photos** — 4 curated Unsplash presets (Mountains, Earth, Abstract, Forest)

**Animated** — 4 pure CSS animated backgrounds:
- Matrix Rain (falling green characters)
- Starfield (twinkling stars)
- Pixel Grid (pulsing colored blocks)
- Synthwave (retro grid + glowing sun)

**Upload** — Supports images, GIFs, and video files (.mp4, .webm)

### Clock Styles
Switch between 4 timer display styles:
- **Minimal** — Clean monospace
- **Digital** — Green LED with glow
- **8-Bit** — Pixelated retro font (Press Start 2P)
- **Flip** — Card-style flip clock digits

## Screenshots

![Customise Modal — Backgrounds tab](/Users/oli/.gemini/antigravity/brain/da4f23d1-8ef8-43b7-8f78-81fa6a861bb8/customise_modal.png)

![Clock Style selector with Digital, 8-Bit, and Flip previews](/Users/oli/.gemini/antigravity/brain/da4f23d1-8ef8-43b7-8f78-81fa6a861bb8/clock_styles.png)

![Full test recording](/Users/oli/.gemini/antigravity/brain/da4f23d1-8ef8-43b7-8f78-81fa6a861bb8/focus_final_test_1772030828709.webp)
