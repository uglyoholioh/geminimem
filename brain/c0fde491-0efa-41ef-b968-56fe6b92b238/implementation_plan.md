# Dashboard Widget Improvements

Standardize widget design, optimize layouts for all supported sizes, and enhance visual feedback for a more premium experience.

## Proposed Changes

### UI Standardization & Polish

- **[MODIFY] [WidgetShell.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/WidgetShell.tsx)**: Refine the shell padding and header styling to be more consistent and premium.
- **Standardize Paddings**: Audit all widgets to use consistent internal padding (e.g., `p-4` or `p-5`) and gap spacing.

---

### Widget-Specific Improvements

#### [MODIFY] [UpcomingDeadlines.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/widgets/UpcomingDeadlines.tsx)
- **New `sm` Layout**: Implement a ultra-compact view for small widgets. Instead of large rows, use a denser list or a horizontal scroll / mini-grid.
- **Enhanced `md`/`lg` Layout**: Show full course names and exact due times. Use a progress indicator for deadlines approaching within 24h.

#### [MODIFY] [QuickAdd.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/widgets/QuickAdd.tsx)
- **Compact `sm` View**: Hide the course selector by default and use a more streamlined input area to fit better in single-column layouts.

#### [MODIFY] [SpotifyMini.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/widgets/SpotifyMini.tsx) & **[SpotifyPlayer.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/SpotifyPlayer.tsx)**
- **Micro-Player for `sm`**: The current vinyl player is too large for an `sm` widget. Implement a "Micro" mode that displays just the album art (mini), track info, and playback controls in a single row or compact stack.

#### [MODIFY] [NowNextClass.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/widgets/NowNextClass.tsx)
- **Refined `sm` View**: Improve typography and add a subtle progress indicator even in the compact view.
- **Visual Polish**: Add a soft glow to the active class indicator.

#### [MODIFY] [SyncStatus.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/widgets/SyncStatus.tsx)
- **Animation Polish**: Improve the sync rotation animation and add a more descriptive success state (e.g., "Last sync: Xm ago").

---

## Verification Plan

### Automated Tests
- Run existing frontend tests to ensure no regressions:
  `npm test frontend/components/dashboard/__tests__`

### Manual Verification
- **Size Interoperability**:
    1. Enter "Edit Mode" on the dashboard.
    2. Resize `UpcomingDeadlines`, `QuickAdd`, and `NowNextClass` to all supported sizes ('sm', 'md', 'lg').
    3. Verify that layouts adapt correctly and no content "breaks" or overflows.
- **Visual Audit**:
    1. Check for spacing consistency between different widgets.
    2. Verify hover states on all interactive elements.
    3. Check the new Spotify "Micro-Player" in a 1x1 ('sm') widget slot.
