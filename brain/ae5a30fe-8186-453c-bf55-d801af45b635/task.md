# AI Task Generation Debugging

- [x] Identify coding-specific endpoint necessity
- [x] Update `ai_assistant.py` and `brief.py` with coding endpoint and `glm-4.5-air`
- [x] Increase timeout to 60s for reasoning models
- [/] Verify fix with `test_flow` script
## Model Switching and Key Preview
- [ ] Add `glm_model` to `User` database model
- [ ] Update Auth schemas and profile routes
- [ ] Update AI service and task generation to use selected model
- [ ] Update frontend AuthContext and SettingsPage
- [ ] Verify model switching and masked key visibility

## Previous Fixes
- [x] Fix Backend NameErrors
- [x] Fix GLM API 404 Endpoint URL
- [x] Implement 20s timeout and fallbacks
- [x] Updated models to glm-4.5/4.6/4.7
- [x] Fix missing user_id filters in backend queries
