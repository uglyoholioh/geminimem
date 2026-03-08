# Spotify & Calendar Integration Fixes

The goal is to resolve the issue where clicking "Connect Spotify" does nothing and to proceed with the planned Google/Apple Calendar integrations.

## User Review Required

> [!IMPORTANT]
> **Missing Credentials**: Spotify integration requires `SPOTIFY_CLIENT_ID` and `SPOTIFY_CLIENT_SECRET`. I've added placeholders to the backend `.env`. You will need to provide these for the connection to work.

> [!WARNING]
> **Popup Blockers**: The Spotify login flow uses a direct redirect or window change. Ensure popups are not blocked if the flow involves a new window.

## Proposed Changes

### [Component] Backend Spotify Router

Fix the OAuth 2.0 flow to correctly handle user-provided credentials and add profile information.

#### [MODIFY] [spotify.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/spotify.py)
- Update `spotify_callback` to use database-stored `client_id` and `client_secret` based on the `state` (user_id) parameter.
- Add `/me` endpoint to fetch user's Spotify display name, image, and account status (Premium).
- Improve error handling for missing/invalid credentials.

---

### [Component] Frontend Spotify Components

Improve the user experience and feedback during and after authentication.

#### [MODIFY] [SpotifyPlayer.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/SpotifyPlayer.tsx)
- Update error messages to direct users to the "Integrations" tab in Settings.
- Add a "Connected User" status to show the Spotify display name once authenticated.
- Improve the "Connect" button state and loading feedback.

#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/settings/page.tsx)
- Ensure the "Connect Spotify Account" button correctly triggers the OAuth flow.
- Show the connected Spotify account name if available.

## Verification Plan

### Automated Tests
- Mock the Spotify token exchange to verify the database fallback logic in `spotify_callback`.

### Manual Verification
1. Enter custom Spotify Client ID/Secret in the Settings > Integrations tab.
2. Click "Connect Spotify Account".
3. Verify successful redirect and token storage in the database.
4. Verify the Focus page shows "Connected as [Name]" and the vinyl player initializes.
