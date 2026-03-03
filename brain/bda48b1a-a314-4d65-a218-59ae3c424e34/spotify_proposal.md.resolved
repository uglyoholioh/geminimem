# Spotify Integration Proposal

We have two main approaches to connect Spotify to your Focus tab, depending on how deep you want the integration to be.

## Option A: Spotify Embed (Iframe Player) - *Recommended for simplicity*
The easiest and most lightweight way to add Spotify to your Focus page is by embedding a mini-player iframe.
*   **Pros**: 
    *   No backend setup or database changes required.
    *   No need to register a Spotify Developer App or manage Client IDs/Secrets.
    *   Works immediately for any user (Premium or Free).
*   **Cons**:
    *   Limited integration: You can't control it programmatically (e.g., auto-pause when the Pomodoro breaks starts).
    *   Users will need to paste a Spotify Playlist/Album link into the Focus settings to render the player.
    *   Aesthetics: We have limited control over the exact styling of the iframe (though we can wrap it in our dark glass containers).

## Option B: Full Spotify Web Playback SDK
This is a deeper integration where we use Spotify's official JavaScript SDK to build custom play/pause/skip buttons that match our exact glassmorphic aesthetic.
*   **Pros**:
    *   100% control over the UI (we use our own icons and colors).
    *   Deep integration possibilities (e.g., automatically pausing music during a "Break" timer).
*   **Cons**:
    *   **Requires Spotify Premium**: The Web Playback SDK *only* works for users with a paid Spotify Premium account.
    *   **Requires Developer Setup**: You will need to go to the Spotify Developer Dashboard, create an App, and give me a `CLIENT_ID` and `CLIENT_SECRET` to add to the `.env` file.
    *   Requires building an OAuth login flow (users click "Log in with Spotify", get redirected, and we store an access token).

---
> [!IMPORTANT]
> **How would you like to proceed?**
> If you want **Option A**, I can implement the Embed Player immediately using the existing dark glass UI to house it.
> If you want **Option B**, please confirm you have a Spotify Premium account, and I will guide you through getting the Developer credentials before I start coding.
