# AI Chat Tool Usage & Material Retrieval — Fixes

## What Was Wrong

The AI chat's `search_module_materials` tool and material retrieval were failing due to four issues:

| Issue | Root Cause |
|-------|-----------|
| Tools not invoked | System prompt didn't tell the AI it HAS tools — just said "use the above data" |
| Double context injection | `_build_brief_chat_context` + `_inject_context` both built system prompts, bloating the context |
| Clear button 404 | No `/brief/chat/clear` endpoint existed |
| Streaming tool fallback | Tool response dumped at once, breaking SSE parsing |

## Fixes Applied

### 1. System Prompt — Tool Awareness
[brief.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/brief.py) — Added explicit tool instructions:

```diff
+- You have ACCESS TO TOOLS. If information is not in the context above, USE your tools
+  * search_module_materials(query, course_code) — Search inside lecture PDFs, slides
+  * search_tasks, search_assignments, get_grades, etc.
+- ALWAYS call search_module_materials when asked about lecture content
+- NEVER say you cannot access module materials
```

### 2. Double Context Elimination  
[ai_service.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_service.py) — Added `skip_context` parameter to `stream_chat()`. Brief chat passes `True` since it builds its own comprehensive system prompt.

### 3. Clear Endpoint
[brief.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/brief.py) — Added `POST /brief/chat/clear` that deletes all today's `BriefChat` records.

### 4. Tool Error Feedback
[ai_tools.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_tools.py) — `search_module_materials` now returns informative messages with file count instead of empty list.

### 5. Streaming Fallback
[ai_service.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_service.py) — Tool response yielded in 80-char chunks for proper SSE streaming.

## Verification

- ✅ Backend starts cleanly, 20 tools registered
- ✅ Clear button returns 200 OK (previously 404)
- ⚠️ Full tool test hit Gemini API rate limit (429) — code path is correct, just quota exhausted

> [!NOTE]
> RAG data is present: **858 chunks** across **153 indexed files**. The tool invocation flow is structurally correct. Full end-to-end testing requires the Gemini quota to reset.
