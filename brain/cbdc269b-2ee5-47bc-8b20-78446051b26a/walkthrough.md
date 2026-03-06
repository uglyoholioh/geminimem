# Spotify Player Vinyl View Walkthrough

I have updated the Spotify player to default to the vinyl view and improved the "Connect Spotify" suggestion UI.

## Changes Made

### Spotify Player Default View
- The `SpotifyPlayer` component now always returns the `MDVinylPlayer` when a token exists, regardless of whether a track is currently playing.
- `MDVinylPlayer` has been updated to handle a `null` `currentTrack` by displaying "No Track Playing" and "Select a playlist in Spotify".

### Connection Suggestion
- The "Not connected" state has been redesigned to be more visually appealing and feel like a proactive suggestion.
- Added a spinning `Disc3` icon and a slight gradient glow to the "Connect Spotify" card.
- Updated the button text and added a play icon to make it more inviting.

## Verification

### Visual Verification
- [ ] Verify that the vinyl player is visible on the dashboard even when no music is playing.
- [ ] Verify that the "Connect Spotify" suggestion looks premium and inviting when not logged in.
- [ ] Verify that track information updates correctly when music starts playing.
