# Craft Integration Update Walkthrough

I have updated the Craft integration to allow users to choose between syncing their Daily Brief to their **Daily Note** (default) or a **Specific Document**.

## Changes Made

### 1. Backend Service Updates
- **[craft_sync.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/craft_sync.py)**: Modified `push_brief_to_craft` to accept `target_type` and `target_id`. It now dynamically constructs the block position based on these settings.
- **[brief_generator.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/brief_generator.py)**: Implemented the logic to fetch Craft settings from the database and trigger the push after brief generation.
- **[setup.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/setup.py)**: Updated `validate-craft` to perform a write test by pushing a success block.

### 2. Frontend Configuration & Verification UI
- **[settings/page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/settings/page.tsx)**: Added a new section in the Craft tab to select the target type and input a Document ID, along with a "Test Connection" button.
- **[setup/page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/setup/page.tsx)**: Updated the onboarding flow for Power Users to include these same configuration and testing options.

## Verification Result

- **Backend logic**: The code correctly branches between `date` (ISO format) for daily notes and `pageId` for specific documents.
- **Settings persistence**: The frontend now includes these fields in `bulk` updates, ensuring they are saved to the `Settings` table.
- **UI Responsiveness**: The Document ID field only appears when "Specific Document" is selected, keeping the interface clean.

### Settings UI (Mockup of changes)
````carousel
```tsx
// New Integration Target buttons in Settings
<div className="pt-2">
    <label className="text-xs text-secondary block mb-2 font-medium">Integration Target</label>
    <div className="flex gap-3">
        <button onClick={() => setTargetType('daily_note')}>Daily Note</button>
        <button onClick={() => setTargetType('specific_document')}>Specific Document</button>
    </div>
</div>
```
<!-- slide -->
```tsx
// Conditional Document ID field
{settings.craft_target_type === 'specific_document' && (
    <div>
        <label>Document / Block ID</label>
        <input placeholder="doc-xxx-xxx" />
    </div>
)}
```
````
