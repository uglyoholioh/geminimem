# Fix AI Chat Tool Usage & Material Retrieval

RAG data exists (858 chunks, 153 indexed files). The issue is in the invocation pipeline.

## Root Causes

1. **Double context injection** — `brief_chat_stream` builds a system prompt via `_build_brief_chat_context()`, then `stream_chat` calls `_inject_context()` which stacks ANOTHER system prompt. This bloats the context window and can confuse model tool routing.
2. **Missing `/clear` endpoint** — frontend calls `DELETE /brief/chat/clear` which returns 404.
3. **Poor streaming tool fallback** — when tool call detected during streaming, the entire sync response is dumped at once, breaking the streaming UX.
4. **Weak `search_module_materials` error feedback** — returns empty list silently; Gemini interprets this as "tool failed" rather than "no matching materials found".

## Proposed Changes

### Eliminate Double Context Injection

#### [MODIFY] [ai_service.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_service.py)
- Add `skip_context` parameter to `stream_chat()` — when True, skip `_inject_context` since caller already injected full context.

#### [MODIFY] [brief.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/brief.py)
- Pass `skip_context=True` when calling `stream_chat` from `brief_chat_stream`, since `_build_brief_chat_context` already provides comprehensive context.
- Add `DELETE /chat/clear` endpoint that deletes all `BriefChat` records for today.

---

### Improve Tool Error Feedback

#### [MODIFY] [ai_tools.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_tools.py)
- When `search_module_materials` returns empty results, include a message about available file count so Gemini can inform the user properly instead of saying "tool failed".

---

### Fix Streaming Tool Fallback

#### [MODIFY] [ai_service.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_service.py)
- In `_stream_gemini`, when detecting a tool call mid-stream, yield the response in chunks instead of all at once so the frontend streaming handler parsess it correctly.

## Verification

- `pytest` — existing tests should still pass
- Manual: send a message asking about module materials and verify the AI uses tools successfully
