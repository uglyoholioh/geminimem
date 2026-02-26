# Implementation Plan - Model Switching and API Key Preview

Allow users to select their preferred GLM model and view a masked preview of their saved API key in the settings page.

## Proposed Changes

### Backend

#### [MODIFY] [user.py](file:///Users/oli/Desktop/LMSManager/backend/app/models/user.py)
- Add `glm_model` column (String, nullable=True, default="glm-4.5-air").

#### [MODIFY] [authentication.py](file:///Users/oli/Desktop/LMSManager/backend/app/routes/authentication.py)
- Update `/me` endpoint to return `glm_model`.
- Update `update_profile` endpoint to accept and save `glm_model`.
- Ensure `redact_api_key` shows the last few characters as requested (it currently shows first 5 and last 5, which is good, but I'll double check the format).

#### [MODIFY] [brief.py](file:///Users/oli/Desktop/LMSManager/backend/app/routes/brief.py)
- Update `generate_ai_summary` to accept `glm_model`.
- In `get_daily_brief`, pass `current_user.glm_model` to the AI generation function.

---

### AI Service

#### [MODIFY] [ai_assistant.py](file:///Users/oli/Desktop/LMSManager/backend/app/services/ai_assistant.py)
- Update code to use the user-selected model if provided.

---

### Frontend

#### [MODIFY] [AuthContext.tsx](file:///Users/oli/Desktop/LMSManager/frontend/lib/contexts/AuthContext.tsx)
- Update `User` interface to include `glm_model` and `glm_key_preview`.

#### [MODIFY] [SettingsPage.tsx](file:///Users/oli/Desktop/LMSManager/frontend/app/settings/page.tsx)
- Add a dropdown (select) for choosing the GLM model.
- Display `glm_key_preview` if `has_glm_key` is true.
- Update the save logic to include `glm_model`.

## Verification Plan

### Automated Tests
- Run `test_flow` script in `brief.py` with different model settings.
- Verify backend endpoints via `curl`.

### Manual Verification
1. Open Settings page.
2. Verify that the API key preview (e.g., `efdb6...lsQT`) is visible if a key is saved.
3. Change the GLM model via the dropdown.
4. Save settings and refresh.
5. Verify the model choice persists.
6. Verify "Generate Tasks" on Dashboard uses the newly selected model.
