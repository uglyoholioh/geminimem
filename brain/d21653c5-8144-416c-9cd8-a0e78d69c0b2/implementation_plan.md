# Implementation Plan - Fix Gemini Client Initialization

The application is failing to initialize the Gemini client because the `google-genai` package (the newer SDK from Google) is missing, while the older `google-generativeai` package is listed in `requirements.txt`. The code in `ai_service.py` specifically uses the new `google-genai` SDK.

## Proposed Changes

### [Component: Backend Dependencies]

#### [MODIFY] [requirements.txt](file:///Users/oli/Desktop/CraftCanvas/backend/requirements.txt)
- Add `google-genai` to the requirements.
- (Optional) Keep or remove `google-generativeai` depending on if other parts of the system use it (unlikely given `ai_service.py` is the main entry point).

### [Component: AI Service]

#### [MODIFY] [ai_service.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_service.py)
- Ensure the import logic is robust and matches the installed package. (The current logic is correct but fails because the package is missing).

## Verification Plan

### Automated Tests
1. **Dependency Installation**: Run `pip install -r requirements.txt` to ensure both packages are present.
2. **Initialization Test**: Run the standalone debug script `/tmp/debug_gemini.py` to verify that the `AIService` can now initialize the `genai.Client`.
   - Command: `PYTHONPATH=./backend python3 /tmp/debug_gemini.py`
3. **Integration Test**: Send a chat message through the frontend or via a direct API call to `POST /api/v1/brief/chat/stream` and verify the stream works without initialization errors.

### Manual Verification
1. Ask the user to try the chat feature again and confirm the error "Gemini client not initialized" no longer appears.
