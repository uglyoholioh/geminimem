# Walkthrough - Resolving Gemini Quota Issues

I have identified and resolved the `RESOURCE_EXHAUSTED` (429) errors you were encountering.

## The Issue
The Gemini API's free tier has specific per-model quotas. Even if you haven't used the individual model much today, certain models (like `gemini-2.0-flash`) can have very restrictive global or account-level limits that trigger 429 errors.

## The Fix
I switched the application to use **`gemini-2.5-flash-latest`** (aliased as `gemini-2.5-flash`). This model has its own separate quota and I have verified it is fully functional for your account.

### Changes Made
1. **`backend/config.py`**: Updated the default fallback models to `gemini-2.5-flash`.
2. **`.env`**: Updated `EXTERNAL_LLM_MODEL` to `gemini-2.5-flash`.

## Verification Results

### Chat Connectivity
Verified that the standard chat now responds instantly using the new model:
```text
AI PROVIDER: gemini
MODEL NAME: gemini-2.5-flash
Testing basic chat...
API TEST SUCCESS: Hello!
```

### Daily Brief Generation
Confirmed that the complex briefing generation (which uses tools to fetch your tasks and timetable) now succeeds using Gemini:
```text
Testing brief generation (includes tools)...
BRIEF SUCCESS! Model used: gemini-2.5-flash
Preview: 📋 Overview Good morning! It looks like you have a wonderfully light day ahead...
```

You are now back online with Gemini 2.5 Flash!
