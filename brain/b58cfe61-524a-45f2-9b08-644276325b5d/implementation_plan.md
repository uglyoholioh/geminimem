# Indicate AI Model Used for Answers

The user wants a clear indication of which AI model is generating responses.

## Proposed Changes

We will stream the model name from the backend before the first chunk of text, parse it in the frontend API client, save it in the chat hook state, and display it in the chat UI.

### Backend

#### [MODIFY] backend/routers/brief.py
- Update `brief_chat_stream` to send the AI model name as a metadata event at the beginning of the stream.
  ```python
  from config import settings
  model_used = settings.gemini_model if settings.ai_provider == "gemini" else settings.ollama_default_model
  yield f"data: {json.dumps({'meta': {'model': model_used}})}\\n\\n"
  ```
- Store `model_used` in the database when saving the assistant's response to `BriefChat` (if applicable and if the schema supports it. If it doesn't support model, we can skip DB persistence for now or add a quick migration. Given the prompt scope, making it visible during chat is priority).

### Frontend

#### [MODIFY] frontend/lib/api.ts
- Modify `stream` to accept an optional `onMeta?: (meta: any) => void` callback.
- Add logic to parse `parsed.meta` and invoke the callback if it exists.

#### [MODIFY] frontend/hooks/useChat.ts
- Update `ChatMessage` interface to include an optional `model?: string` field.
- Pass the `onMeta` callback to `api.stream` to update the `model` property of the last inserted assistant message state.

#### [MODIFY] frontend/components/chat/DailyBriefChat.tsx
- Display the model name below the assistant's message, next to the timestamp. We'll use a small text styled with "font-mono text-[10px]" similar to the timestamp, accompanied by a subtle icon (e.g., `<Sparkles />`).

## Verification Plan

### Automated Tests
- N/A - No specific automated tests for this exact functionality.

### Manual Verification
1. Open the app on the browser.
2. Go to the dashboard or right sidebar chat.
3. Send a message to the AI.
4. Verify that the response text streams in as usual and the model name (e.g., `gemini-1.5-pro` or `gemini-2.5-flash` or whatever is configured) appears immediately at the bottom of the chat bubble.
5. Check backend logs to ensure no streaming errors.
