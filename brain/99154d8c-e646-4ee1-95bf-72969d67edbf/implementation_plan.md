# Integrate Gemini AI Studio Models

This plan outlines the integration of Gemini models from Google AI Studio into the CraftCanvas backend. This provides a high-performance alternative to local Ollama.

## User Review Required

> [!IMPORTANT]
> **Data Privacy**: In the Gemini AI Studio **Free Tier**, Google may use your data (inputs and outputs) to train and improve their models. Please ensure this aligns with your privacy requirements.
> 
> **API Key**: You will need to obtain an API key from [aistudio.google.com](https://aistudio.google.com/) and add it to your `.env` file as `GOOGLE_API_KEY`.

## Proposed Changes

### Backend Configuration

#### [MODIFY] [config.py](file:///Users/oli/Desktop/CraftCanvas/backend/config.py)
- Add `google_api_key` to `Settings`.
- Add `ai_provider` setting to toggle between `ollama` and `gemini`.
- Add `gemini_model` for general chat (default: `gemini-1.5-flash`).
- Add `gemini_pro_model` for high-accuracy tasks (default: `gemini-1.5-pro`).

### AI Service Implementation

#### [MODIFY] [ai_service.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_service.py)
- Refactor `AIService` to support multiple providers.
- Implement model-specific routing based on `task_type`:
    - `brief_generation` and `parse` tasks will use `gemini_pro_model` (Pro) for higher accuracy.
    - General `chat` tasks will use `gemini_model` (Flash).

### Dependencies

#### [MODIFY] [requirements.txt](file:///Users/oli/Desktop/CraftCanvas/backend/requirements.txt)
- Add `google-generativeai` library.

## Cost Estimation (Gemini AI Studio Free Tier)

| Model | RPM | RPD | TPM | Cost |
| :--- | :--- | :--- | :--- | :--- |
| **Gemini 1.5 Flash** | 15 | 1,500 | 1,000,000 | **$0** |
| **Gemini 1.5 Pro** | 2 | 50 | 32,000 | **$0** |

**Budget Strategy**: By routing only critical tasks (generation, parsing) to Pro and standard chat to Flash, we stay within the daily Pro limit (50 requests/day) while benefiting from high accuracy where it matters most, all at zero cost.

## Verification Plan

### Automated Tests
- Updated `test_gemini.py` to verify that `task_type="brief_generation"` correctly routes to the Pro model and standard chat routes to the Flash model.

### Manual Verification
- Toggle `AI_PROVIDER=gemini` in `.env`.
- Generate a Daily Brief and verify it uses the Pro model (check logs).
- Use the Chat feature and verify it uses the Flash model.
