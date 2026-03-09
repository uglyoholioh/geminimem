# Focus UI Refinement & Sidebar Toggle

The goal is to fix the settings modal cropping on the Focus page and provide a way to toggle the sidebar on/off while in Focus mode, ensuring the immersive feel is preserved but navigation is still accessible.

## Proposed Changes

### Global Layout

#### [MODIFY] [ClientLayout.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/ClientLayout.tsx)

- **Manage Sidebar Visibility**: Add a state or check for a toggle (potentially using local storage or a URL param) to hide/show the sidebar specifically on the Focus page.
- **Restore Default Main Structure**: Ensure the main area correctly handles the sidebar's presence/absence without breaking the full-screen background of the Focus page.

### Focus Page

#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/focus/page.tsx)

- **Fix Modal Cropping**: The `overflow-hidden` on the main card container (around line 835) is likely causing the settings modal to be clipped. I'll move the modal outside this container or adjust the overflow properties.
- **Add Sidebar Toggle Button**: Place a subtle toggle button (e.g., in the bottom left or as part of the header area) to trigger the sidebar visibility.

## Verification Plan

### Manual Verification
- **Sidebar Toggle**: Verify that the sidebar can be hidden and shown smoothly on the Focus page.
- **Modal Visibility**: Check the settings customise modal to ensure it's fully visible and not cropped by the glass card.
- **Layout Integrity**: Ensure backgrounds still fill the screen regardless of sidebar state.
