# Fix AI Tool Capabilities

## Planning
- [x] Analyze root causes of tool failures (serialization, ContextVar, RAG)
- [x] Write implementation plan

## Execution
- [x] Add `_safe_serialize()` helper to `ai_tools.py`
- [x] Apply ContextVar fallback to all 18 tool functions
- [x] Add `due_time` support to `create_task` tool
- [x] Apply `_safe_serialize()` to all tool return values
- [x] Clean up debug prints in `ai_service.py` and `ai_tools.py`
- [x] Restart backend

## Verification
- [x] Test task creation (serialization fix)
- [x] Test assignment queries (ContextVar streaming fix)
- [x] Test canvas material retrieval (RAG)
- [x] Verify `due_time` correctly saved and serialized
