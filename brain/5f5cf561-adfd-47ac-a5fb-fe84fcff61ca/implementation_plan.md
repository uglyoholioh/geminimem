# Craft Integration Update Plan

Currently, the Craft integration conceptually targets the "Daily Note" but doesn't fully implement the push logic within the daily brief generation cycle, and only collects the connection URL and API key from the frontend. 

## Proposed Changes

### 1. `backend/services/craft_sync.py`
- Modify `push_brief_to_craft` to accept `target_type: str = "daily_note"` and `target_id: str = ""`.
- If `target_type == "daily_note"`, format the payload position as `{"date": brief.date.isoformat(), "position": "end"}` (existing behaviour).
- If `target_type == "specific_document"`, format the payload position as `{"pageId": target_id, "position": "end"}` if `target_id` is provided, otherwise `{"index": "end"}` to append to the document linked directly by the connection URL.

### 2. `backend/services/brief_generator.py`
- At the bottom of `generate_brief`, implement the missing logic to fetch the Craft settings (`craft_api_url`, `craft_api_token`, `craft_target_type`, `craft_target_document_id`) from the `Settings` table.
- Use these settings to actually invoke `await push_brief_to_craft(...)`.
- We'll need to import the `Settings` model correctly: `from models.settings import Settings`.

### 3. `frontend/app/settings/page.tsx`
- Add `craft_target_type` (default `'daily_note'`) and `craft_target_document_id` (default `''`) to `AppSettings` interface and `DEFAULT_SETTINGS`.
- Add a UI section in the Craft tab to let users choose between "Daily Note" (default) and "Specific Document".
- If "Specific Document" is selected, show an input field for the Document ID.

### 4. `frontend/app/setup/page.tsx`
- Add state variables for `craftTargetType` and `craftTargetId`.
- Update the UI in Step 4 under Craft to match the settings page, allowing users to configure this during setup.
- Include these new keys in the `settingsData` object sent to `/settings/bulk`.

### 5. `backend/routers/setup.py`
- Update `validate-craft` to handle `target_type` and `target_id`.
- Instead of just checking `/folders`, attempt to push a "Test Connection" block to the selected target to verify write access.
- Update `CraftWorkspaceValidationRequest` to include `target_type` and `target_id`.

## Verification Plan

### Automated Tests
- Since this relies on an external API (Craft), we'll do manual verification through the browser.

### Manual Verification
- Test the "Test Connection" button in both Setup and Settings pages.
- Verify that a block "✅ CraftCanvas: Connection successful!" appears in the selected Craft target (Daily Note or Specific Doc).
