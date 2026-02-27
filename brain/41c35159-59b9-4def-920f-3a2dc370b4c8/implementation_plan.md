# Tag Colour Coding Implementation Plan

This plan outlines how to implement colour coding for task tags, with automatic defaults and user customisation.

## Proposed Changes

### Backend

#### [MODIFY] [backend/routers/settings.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/settings.py)
- Update `get_all_settings` to include a default `tag_colours` mapping if not present.
- Default mapping will cover the initial preset tags with distinct colors.

### Frontend

#### [MODIFY] [frontend/app/tasks/page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/tasks/page.tsx)
- Fetch `tag_colours` from settings and store in state.
- Create a `getTagStyle(tag: string)` helper:
    - Returns a color and a semi-transparent background color.
    - If tag is in `tag_colours`, use that.
    - If not, deterministically generate a color based on the tag name (hash string to HSL).
- Update `TaskRow`, `InlineTaskCreator`, and `TaskDetail` to use these styles for tags.
- In `TaskDetail`, add a small color picker (or a set of logical preset colors) next to each tag in the detail view to allow customisation.
- Customisation will save the new color to the `tag_colours` setting via the bulk update API.

## Verification Plan

### Manual Verification
- Verify that default tags have consistent colors.
- Create a new custom tag and verify it gets an automatically assigned color.
- Change a tag's color in the detail view and verify it updates across the UI and persists after refresh.
