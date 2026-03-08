# Implementation Plan: Fix Spotify Player 404 and TypeError

The Spotify player is failing with a 404 error because the backend is running a stale version of the code where the Spotify router is either not registered or the reloader has stopped due to a previous import error. Additionally, there is a `TypeError` in the Spotify router when comparing datetimes.

## Proposed Changes

### Backend

#### [MODIFY] [spotify.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/spotify.py)
- Use `lib.timezone.now_utc()` instead of `datetime.now(UTC)` to ensure consistency with the rest of the project (using naive UTC datetimes).
- Fix the comparison in `get_spotify_token` to avoid `TypeError`.

#### [EXECUTE] Restart Backend Server
- Kill the current `uvicorn` process.
- Restart `uvicorn` to ensure all routers (including Spotify) are correctly registered and the reloader is active.

## Verification Plan

### Automated Tests
- Run `curl -i http://localhost:8000/api/v1/spotify/me` to verify that it returns `401 Unauthorized` (or `200 OK` if a session is provided) instead of `404 Not Found`.

### Manual Verification
- Refresh the frontend and check the browser console to confirm the 404 error is gone.
- Verify the Spotify player loads (it should show "Connect Spotify" if not authenticated, or the player if it is).
