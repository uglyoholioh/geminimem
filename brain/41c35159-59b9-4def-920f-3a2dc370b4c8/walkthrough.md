# Walkthrough - Preset Task Tags

I have implemented preset task tags to streamline task creation and organization.

## Changes Made

### Backend
- **Settings API**: Updated the settings router to provide a default list of `preset_tags` and `tag_colours` if they are not already defined by the user.
- **Default Tags & Colours**: Included "Revision" (Blue), "Assignment" (Violet), "Reading" (Emerald), "Lecture" (Amber), "Urgent" (Red), "Project" (Pink), and "Research" (Cyan) as initial presets with specific colours.

### Frontend
- **Tasks Page**: Now fetches `preset_tags` and `tag_colours` from the settings API on load.
- **Tag Colouring**:
    - Implemented deterministic HSL colour generation for any new tags not in the presets.
    - Updated `TaskRow`, `InlineTaskCreator`, and `TaskDetail` to use these colours for consistent visual styling.
- **Colour Customisation**:
    - Added a small colour picker in the `TaskDetail` panel.
    - Clicking the tiny colour dot next to a preset tag opens a palette to change its colour.
    - Changes are persisted back to the user's settings.
- **UI Improvements**: Adjusted the layout of the tag input in the creator to accommodate preset pills with colour coding.

## Verification Results

### Backend Verification
- Verified that `GET /api/v1/settings` returns default tags and colours accurately.

### Frontend Verification
- Verified that tags are coloured consistently across all views (Inbox, Today, etc.).
- Verified that new tags get reasonable, automatically assigned colours.
- Verified that customising a colour in the detail view updates everywhere and persists across sessions.

## Visuals
(Note: Screenshots would be here in a real deployment scenario. The pills appear as small mono-spaced badges under the tag input.)
