# Relocate Canvas Sync Status Indicator

This plan outlines the steps to remove the "Canvas Synced" button from the sidebar and relocate it to the top of the dashboard header, alongside existing status badges. The new indicator will be more subtle and consistent with the dashboard's aesthetic.

## Proposed Changes

### Frontend Components

#### [MODIFY] [SyncStatusIndicator.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/SyncStatusIndicator.tsx)
- Refactor to support a more compact, badge-like appearance.
- Simplify the layout to match the `StatBadge` style used in the `DashboardHeader`.
- Remove the background and border to make it "subtle" as requested.

#### [MODIFY] [Sidebar.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/layout/Sidebar.tsx)
- Remove the `SyncStatusIndicator` component and its import.
- Remove the separator line above the theme toggle if no longer needed.

#### [MODIFY] [DashboardHeader.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/DashboardHeader.tsx)
- Import and integrate `SyncStatusIndicator` into the right-side actions area.
- Ensure it aligns perfectly with existing badges (`tasks`, `classes`, `min`, `due`).

## Verification Plan

### Manual Verification
- **Visual Check**: Open the dashboard and verify that the sync status appears to the left or right of the existing badges in the header.
- **Subtlety Check**: Confirm that the indicator is less prominent than it was in the sidebar (less "button-like").
- **Functionality Check**: Verify that the sync status still updates (e.g., shows "Syncing..." when a sync is triggered or the relative time updates).
- **Sidebar Check**: Confirm the sidebar no longer contains the sync indicator.
- **Mobile/Responsive**: Check if the header remains clean on smaller screens with the additional badge.
