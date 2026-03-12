# Spotify Connection Button Fix

The goal is to ensure that the "Connect Spotify" button is always visible when the user is not connected, even if Spotify credentials (Client ID/Secret) are not yet configured in the environment or database. Additionally, clicking "Connect Spotify" should initiate the OAuth flow directly if possible, or provide clear guidance, rather than just stating that configuration is required.

## Proposed Changes

### Frontend

#### [MODIFY] [SpotifyPlayer.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/SpotifyPlayer.tsx)
- Relax the error check that hides the connect button.
- Instead of showing a static "Configuration Required" message when credentials are missing, show the "Connect Spotify" button.
- If the backend returns a 400 error for `/spotify/login` (missing credentials), handle it gracefully by showing a helpful message to the user instead of just failing.

### Backend

#### [MODIFY] [spotify.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/spotify.py)
- Optional: Ensure the `/spotify/login` endpoint provides a clear error message that the frontend can display. (Current implementation already raises a 400 with a detail message).

## Verification Plan

### Manual Verification
1. **Scenario: Credentials Missing**
   - Ensure `SPOTIFY_CLIENT_ID` and `SPOTIFY_CLIENT_SECRET` are NOT set in `.env`.
   - Open the Focus page.
   - Verify that the "Connect Spotify" button is visible and NOT replaced by a "Configuration Required" static text.
   - Click "Connect Spotify".
   - Verify that an alert or toast appears explaining that the administrator needs to configure Spotify, instead of a silent failure or broken link.
2. **Scenario: Credentials Present**
   - Set valid (or dummy) `SPOTIFY_CLIENT_ID` and `SPOTIFY_CLIENT_SECRET` in `.env`.
   - Open the Focus page.
   - Click "Connect Spotify".
   - Verify it redirects to Spotify's authorization page.
