# Fix: Courses Page Refresh Glitch & CORS Errors

## Problem
The `/courses` page showed a "refresh-like" glitch with console CORS errors on `/assignments`, `/materials`, and `/announcements`. Stats showed 0 for Tasks, Files, and News.

## Root Causes Found

### Primary: Cache serialization mismatches → 500 errors → no CORS headers

The **real** root cause: the cache serialization in route handlers produced dicts with wrong/missing field names, causing FastAPI response validation to fail with **500 Internal Server Error**. Starlette's default 500 handler runs *outside* the CORS middleware, so the browser sees a response without `Access-Control-Allow-Origin` and reports a CORS error — hiding the actual 500.

| File | Issue |
|------|-------|
| [materials.py](file:///Users/oli/Desktop/LMSManager/backend/app/routes/materials.py) | Cache used `filename` instead of `file_name`; missing `module_name`, `description`, `download_url` |
| [announcements.py](file:///Users/oli/Desktop/LMSManager/backend/app/routes/announcements.py) | Cache missing `author_name` field |

### Secondary: Middleware & fetcher improvements

| File | Issue |
|------|-------|
| [main.py](file:///Users/oli/Desktop/LMSManager/backend/app/main.py) | `RequestIDMiddleware` was outermost, could mask CORS headers on crash |
| [logging.py](file:///Users/oli/Desktop/LMSManager/backend/app/middleware/logging.py) | Used global `setLogRecordFactory()` — unsafe for concurrent async requests |
| [index.ts](file:///Users/oli/Desktop/LMSManager/frontend/lib/hooks/index.ts) | `Content-Type: application/json` on GET requests forced unnecessary CORS preflight |

## Changes Made

render_diffs(file:///Users/oli/Desktop/LMSManager/backend/app/routes/materials.py)

render_diffs(file:///Users/oli/Desktop/LMSManager/backend/app/routes/announcements.py)

render_diffs(file:///Users/oli/Desktop/LMSManager/backend/app/main.py)

render_diffs(file:///Users/oli/Desktop/LMSManager/backend/app/middleware/logging.py)

render_diffs(file:///Users/oli/Desktop/LMSManager/frontend/lib/hooks/index.ts)

## Verification

````carousel
![Before — all stats at 0, CORS errors in console](/Users/oli/.gemini/antigravity/brain/c825c9b8-902b-4774-8f59-a81660e81bf2/courses_page_verification_1771420276507.png)
<!-- slide -->
![After — 8 courses, 10 pending, 9 materials, proper per-course counts](/Users/oli/.gemini/antigravity/brain/c825c9b8-902b-4774-8f59-a81660e81bf2/final_verification_1771420858949.png)
````

- **Backend curl tests**: All endpoints return 200 with CORS headers ✅
- **Browser verification**: Stats show real data (8 courses, 10 pending, 9 materials) ✅
- **No flickering**: Page remains stable ✅
