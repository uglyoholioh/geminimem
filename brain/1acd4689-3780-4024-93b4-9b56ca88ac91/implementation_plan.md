# Immersive Vinyl Player for Focus Tab

Transform the small inline Spotify player into a large, visually rich, almost tactile vinyl record experience that becomes the centrepiece of the Focus tab when music is playing.

## Proposed Changes

### Vinyl Player Component

#### [MODIFY] [SpotifyPlayer.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/SpotifyPlayer.tsx)

Complete redesign of the active-player UI section. The "Connect Spotify" and "Spotify Connected" states remain the same (compact cards). When music is actively playing, the component renders the new immersive vinyl experience:

**Visual design — Large vinyl record (280–320px):**
- Album art fills the center label area (~45% of the disc)  with a circular clip
- Realistic vinyl grooves rendered as concentric semi-transparent rings  
- A dark outer rim with subtle radial gradient simulating the vinyl edge
- Glossy highlight / light streak across the surface (CSS pseudo-element, angled linear gradient at ~10% opacity)
- **Spin animation**: smooth 4s infinite rotation when playing; pauses via `animation-play-state: paused` when paused — preserving the current angle
- Centre spindle: small amber dot with subtle glow

**Tone arm:**
- SVG-based tone arm positioned to the upper-right of the record
- Pivots from a fixed point; rotates ~20° inward when playing (CSS `transform: rotate()` with transition)
- When paused, arm lifts back to resting position  
- Subtle drop-shadow under the arm for depth

**Controls — minimal, underneath the record:**
- Previous / Play-Pause / Next in a horizontal row
- Play/Pause is a larger circle button with amber accent
- Skip buttons are smaller, muted

**Track info — below controls:**
- Song title (DM Sans, 13px, `--text-primary`)
- Artist (DM Mono, 10px uppercase, `--text-muted`)
- Marquee scroll for long titles (existing behaviour preserved)

**Micro-interactions:**
- Hover on the vinyl: very subtle scale-up (1.02) 
- Clicking anywhere on the vinyl disc toggles play/pause
- Needle drop sound-effect feeling via a brief CSS "bounce" when play begins  
- Ambient glow behind the record (blurred album art colour, ~10% opacity radial)

---

### Focus Page Layout Integration

#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/focus/page.tsx)

When Spotify is enabled **and** a track is actively playing, the vinyl player gets promoted to a prominent position:

- The `SpotifyPlayer` component is moved from the bottom of the right panel into the **left column** (timer side), rendered **below the timer controls** and **above the session stats**
- It replaces the cramped bottom-of-panel position with a centred, breathing layout
- When no track is playing (or Spotify is disabled), the layout remains exactly as-is

This is achieved by passing an `isLargeMode` prop to `SpotifyPlayer` — when `true`, the immersive vinyl renders; when `false`, the compact card/connect states render in the right panel as before.

## Verification Plan

### Manual Verification (browser)
1. Run `cd /Users/oli/Desktop/CraftCanvas/frontend && npm run dev`
2. Open `http://localhost:3000/focus` in the browser
3. **Without Spotify connected**: Confirm the Focus page looks identical to before — timer on left, tasks on right, no vinyl player visible
4. **Enable Spotify in Customise → Sounds**: Toggle on — the compact "Connect Spotify" or "Spotify Connected" card should appear in the right panel bottom, same as before
5. **With music playing**: The large vinyl record should appear in the left column below the timer. Verify:
   - Record spins smoothly when playing
   - Record pauses (freezes angle) when paused
   - Tone arm animates in/out
   - Album art visible in the centre
   - Clicking the record toggles play/pause
   - Controls (prev/play/next) work
   - Track title and artist display correctly
6. **Responsive check**: Ensure the layout doesn't overflow on smaller viewports (the vinyl should scale down gracefully)
