# Remove Living Planner and Evening Reflection

The user wants to simplify the planner by removing the "Living Planner" sidebar and the "Evening Reflection" functionality. This involves editing the main `PlannerPage` component and cleaning up its dependencies.

## Proposed Changes

### [Component Name] Frontend - Planner Page

#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/planner/page.tsx)
- Remove the `aside` element containing the "Living Planner" header, `CompanionPanel`, and "Daily Nurture" section.
- Remove the `EveningReflection` modal component.
- Remove state variables: `showReflection`, `companionData`.
- Remove `CompanionPanel` and `EveningReflection` imports.
- Adjust the main layout to ensure the timeline takes up the full width (or stays centered appropriately).
- Remove the logic that fetches `companion/state`.

## Verification Plan

### Manual Verification
- Navigate to `/planner`.
- Verify that the right sidebar titled "Living Planner" is no longer visible.
- Verify that the "Evening Reflection" button is gone.
- Verify that the layout remains responsive and the timeline view still functions correctly.
- Check the browser console for any errors related to removed components or state.
