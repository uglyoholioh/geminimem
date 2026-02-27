# Gemini AI Studio Integration Walkthrough

I have integrated the free tier Gemini models from Google AI Studio into CraftCanvas. This allows you to leverage powerful models like Gemini 1.5 Flash for chat and daily briefs.

## Changes Made

### Backend

- **[config.py](file:///Users/oli/Desktop/CraftCanvas/backend/config.py)**: Added settings for `GOOGLE_API_KEY`, `AI_PROVIDER`, `GEMINI_MODEL` (Flash), and `GEMINI_PRO_MODEL` (Pro).
- **[ai_service.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_service.py)**: Refactored `AIService` to support multiple providers and model-specific routing.
    - **Gemini 1.5 Pro**: Used for `brief_generation` and `parse` tasks to ensure high accuracy for data extraction.
    - **Gemini 1.5 Flash**: Used for general `chat` requests to provide fast, responsive interaction.
- **[requirements.txt](file:///Users/oli/Desktop/CraftCanvas/backend/requirements.txt)**: Added `google-generativeai` package.

## Cost & Accuracy Strategy (Zero Cost)

By using this hybrid approach in the **Gemini AI Studio Free Tier**, the app maximizes accuracy where it counts (like extracting task dates and generating briefs) while staying within the free tier limits:
- **Gemini 1.5 Flash**: 1,500 requests/day (Free)
- **Gemini 1.5 Pro**: 50 requests/day (Free)

This setup ensures the highest intelligence for core features at **$0 cost** to you.

## Verification Results

### Automated Tests
- Updated [test_gemini.py](file:///Users/oli/Desktop/CraftCanvas/backend/tests/test_services/test_gemini.py) to verify:
    - `task_type="brief_generation"` correctly routes to **Gemini 1.5 Pro**.
    - Standard chat correctly routes to **Gemini 1.5 Flash**.
    - Successful streaming of responses.
- **Result**: `3 passed`

## How to use Gemini

1.  **Add your API Key**: Go to [aistudio.google.com](https://aistudio.google.com/), generate an API key, and add it to your `.env` file:
    ```bash
    GOOGLE_API_KEY=your_key_here
    ```
2.  **Switch Provider**: Update your `.env` to use Gemini:
    ```bash
    AI_PROVIDER=gemini
    ```
3.  **Restart the backend**.

Ollama will still be used for local embeddings to ensure your vector store remains consistent and private.
