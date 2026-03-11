# Fix Spotify API 500 Error

The backend is failing with a `NameError: name 'UTC' is not defined` in the Spotify router. This happens because `UTC` is imported as `from datetime ... UTC`, but the code uses it directly in some places while potentially expecting it from `datetime.UTC` (which is available in Python 3.11+). However, the traceback shows it's being used as `now_utc()` or `datetime.now(UTC)` and failing.

Actually, looking at the imports:
`from datetime import datetime, timedelta, UTC`

Wait, line 151 says:
`new_expires = datetime.now(UTC) + timedelta(seconds=token_data["expires_in"])`

If `UTC` is imported from `datetime`, it should be available. Oh, wait! `UTC` was added to `datetime` in Python 3.11. If the environment is older, it might fail, but the user is on Mac and likely using a recent Python.

Actually, let's look at the imports in `spotify.py` again:
`from datetime import datetime, timedelta, UTC`

If this import works, `UTC` is in the local namespace.
But the error says:
`File "/Users/oli/Desktop/CraftCanvas/backend/routers/spotify.py", line 151, in get_spotify_token`
`new_expires = datetime.now(UTC) + timedelta(seconds=token_data["expires_in"])`
`NameError: name 'UTC' is not defined`

This is strange. Let me double check the file content.
Ah! I see `from datetime import datetime, timedelta, UTC` at line 7.
But maybe it's NOT at line 7 in the actual running code?

Wait, I also see `from lib.timezone import now_utc` at line 13.

Let's look at the `latest_server.log` again.
`File "/Users/oli/Desktop/CraftCanvas/backend/routers/spotify.py", line 151, in get_spotify_token`
`new_expires = datetime.now(UTC) + timedelta(seconds=token_data["expires_in"])`

Wait, when I read the file via `view_file`, line 151 was:
`151:             _save_setting(session, current_user.id, "spotify_token_expires_at", new_expires.isoformat())`

Wait, the line numbers in the log don't match my `view_file` output!
In `view_file` output:
`150:             new_expires = now_utc() + timedelta(seconds=token_data["expires_in"])`
`151:             _save_setting(session, current_user.id, "spotify_token_expires_at", new_expires.isoformat())`

But the log says:
`File "/Users/oli/Desktop/CraftCanvas/backend/routers/spotify.py", line 151, in get_spotify_token`
`new_expires = datetime.now(UTC) + timedelta(seconds=token_data["expires_in"])`

This means the file ON DISK changed or I misread it, or the server is running an older version of the file? No, uvicorn reloader is active.

Let me check `spotify.py` again VERY CAREFULLY.

Line 150 in my `view_file` output:
`150:             new_expires = now_utc() + timedelta(seconds=token_data["expires_in"])`

Wait, `now_utc` is imported from `lib.timezone`.
Let me check `lib/timezone.py`.

## Proposed Changes

### Backend

#### [MODIFY] [spotify.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/spotify.py)
- Ensure consistent use of `now_utc()` or fix the `UTC` import.
- The log shows `datetime.now(UTC)` being used, but my `view_file` showed `now_utc()`. This is a discrepancy. I will unify it to use `now_utc()` which seems to be the project standard.

## Verification Plan

### Automated Tests
- None, manual verification preferred.

### Manual Verification
- Restart backend (should happen automatically via uvicorn).
- Refresh the dashboard and check if Spotify player loads without 500 error.
