# Fix AI Tool Capabilities

Comprehensive fix for AI tool interactions including task creation, scheduling, and canvas material retrieval.

## Root Causes

1. **JSON Serialization**: `task.dict()` returns Python `date`/`datetime` objects that can't be serialized by the Gemini SDK → `Object of type date is not JSON serializable`
2. **ContextVar Gap**: Only `create_task` reads from `ContextVar` as fallback. All other tools silently get `session=None` in streaming mode → tools crash or return empty
3. **Canvas Retrieval**: Many files failing text extraction (CSVs, images, .doc, .pptx) — need to verify RAG pipeline works for indexed PDFs

## Proposed Changes

### AI Tools Component

#### [MODIFY] [ai_tools.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_tools.py)

1. **Add `_safe_serialize()` helper** — recursively convert `date`/`datetime` to ISO strings in dicts/lists
2. **Apply ContextVar fallback to ALL 18 tools** — change every `kwargs.get('session')` to `kwargs.get('session') or ai_session.get()` (same for `user_id`)
3. **Use `_safe_serialize()` on all return values** that include `.dict()` output
4. **Remove debug prints** — keep only `logger` calls

---

#### [MODIFY] [ai_service.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_service.py)

1. **Remove debug prints** — clean up `print(f"DEBUG: ...")` statements
2. **Keep manual tool loop in `_call_gemini`** — still used for brief generation and non-streaming calls
3. **Keep `automatic_function_calling=True` in `_stream_gemini`** — works with ContextVars

## Verification Plan

### Automated Tests
1. Restart backend, use browser to ask AI: "Add a task: Test serialization fix by Friday"  
2. Ask AI: "What are my upcoming assignments?" (tests `search_assignments` with ContextVar)
3. Ask AI: "What topics are covered in BT1101 lectures?" (tests `search_module_materials` RAG)
