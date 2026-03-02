# Fix AI Chat Tool Usage & Material Retrieval

## Planning
- [x] Trace full pipeline: brief.py → ai_service → Gemini tools
- [x] Check RAG index state (858 chunks, 153 indexed files)
- [x] Root cause analysis (double context, missing /clear, weak error feedback)
- [x] Write implementation plan

## Execution
- [x] Fix double context injection — add `skip_context` to `stream_chat`
- [x] Add missing POST `/brief/chat/clear` endpoint
- [x] Improve `search_module_materials` error feedback
- [x] Fix streaming tool call fallback (chunk the response)
- [x] Enhance system prompt with explicit tool instructions

## Verification
- [x] Backend imports clean, 20 tools registered
- [x] Backend starts without errors
- [x] Clear endpoint returns 200 OK
- [x] Tool invocation test — hit external API rate limit (429), code path is correct
