# Math Lesson Visualizer Verification Checklist

- [X] 1. Initial State (Step 1 of Ortho lesson)
    - [X] UI is off-screen (invisible) by default.
    - [X] When forced visible via JS: Title and Description are correct.
    - [X] Visualization (grid/vectors) is visible and looks good in Step 1.
- [X] 2. Step 2 (Ortho lesson)
    - [X] Visuals break (SVG canvas explosions to millions of pixels).
    - [X] Transition is not smooth due to immediate visual disappearance.
- [X] 3. Step 3 (Casting a Shadow)
    - [X] Visuals remain broken (canvas exploded).
- [X] 4. Step 4 (Least Squares)
    - [X] Visuals remain broken.
- [X] 5. Switch to QR Factorization
    - [X] Loads properly from clean state (reload + switch).
    - [X] Grid and vectors are visible.
- [X] 6. QR Lesson Progression
    - [X] Fails on moving to Step 2 (Canvas explosion).
- [X] 7. Auto-advance (Play button)
    - [X] Triggering Next/Play via JS leads to immediate canvas explosion.

## SUMMARY OF FAILURES:
1. **Critical Layout Bug**: The UI controls and overlay are positioned at negative/large coordinates (y ~ 1200, x ~ -700) making them invisible to the user.
2. **Critical Visualization Bug**: Moving to ANY next step causes the SVG `viewBox` and dimensions to explode (NaN or Infinity in tweening?).
3. **Axes Color**: Grid axes are visible but not "bright white".
4. **Overlay Animation**: Hard to judge due to position issues, but description content does update.

## DIAGNOSIS:
- Camera system or `useTween` hook is producing extreme values during transitions.
- `index.css` or `Player.tsx` has broken absolute/relative positioning for UI layers.
