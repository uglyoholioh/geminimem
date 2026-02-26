# Craft Integration Updates

## 1. Backend Updates
- [x] Update `backend/services/craft_sync.py` to support `target_type` and `target_document_id`.
- [x] Update `backend/services/brief_generator.py` to retrieve the `craft_*` settings from DB and orchestrate the push to Craft using these parameters.
- [x] Update `backend/routers/setup.py` to implement a better `validate-craft` that tests write access by pushing a test block.
- [x] Update `frontend/app/settings/page.tsx` default settings to include `craft_target_type` ("daily_note" or "specific_document") and `craft_target_document_id` (empty by default).
- [x] Add radio buttons or a select dropdown for Craft Target Type in the Craft Integration section of Settings.
- [x] Render a text input for Document ID unconditionally when "specific_document" is selected.
- [x] Add a "Test Connection" button to the Craft settings tab.
- [x] Update `frontend/app/setup/page.tsx` state to include `craftTargetType` and `craftTargetId`.
- [x] Show the selection for target type and optional Document ID in the Power User Integrations step (if Craft connection URL is provided, or always alongside it).
- [x] Ensure these fields are sent in the `bulk` settings updates during onboarding completion.
- [x] Add a "Test Connection" button to the Craft setup step.
- [x] Ensure no compilation errors in frontend.
- [x] Start backend and verify brief generator doesn't crash on manual trigger or scheduler initialization related to Craft variables.
