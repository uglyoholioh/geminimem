# Fix Courses Page Refresh Glitch & Console Errors

## Root Cause
The courses page has CORS errors on `/assignments`, `/materials`, and `/announcements` endpoints.  
SWR retries these failed requests every 2 seconds, causing UI elements to flicker/reset.

## Tasks

- [x] Investigate and identify root cause
- [x] Fix backend middleware ordering (`RequestIDMiddleware` wrapping `CORSMiddleware`)
- [x] Fix `RequestIDMiddleware` thread-safety issue with `logging.setLogRecordFactory()`
- [x] Fix frontend fetcher: remove unnecessary `Content-Type: application/json` on GET requests
- [ ] Verify fix by observing the courses page in the browser
