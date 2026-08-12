Please make the following changes to the app while preserving all existing functionality:

1. Playback context
When I tap a song inside a playlist, album, or Songs folder, start playback at that exact song while keeping the entire playlist/album/Songs collection as Spotify’s playback context, so when the song finishes, Spotify continues with the following songs normally. Use Spotify’s supported context + offset playback functionality; do not increase API requests, add polling, or undo any existing caching, request deduplication, rate-limit handling, or HTTP 429 backoff. And whenever I select any song or playlist episode to play, bring me to the now playing view.

2. Now Playing
If nothing is currently playing when I open Now Playing, first check Spotify’s current playback state and, if Spotify has a paused track, resume that existing playback instead of doing nothing.

3. UI changes

* Change the main UI background from the current blue-tinted background to a normal clean white background.
* Show album artwork/thumbnails next to songs in both playlists and albums.
* Show the artist image next to each artist in the Artists section.
* Make it so holding down the Play/Pause button anywhere in the app opens the Now Playing screen; a normal tap should continue to work exactly as it does now.
* In full-screen Now Playing mode, make the control wheel larger and perfectly centered, with proportions that properly fit the screen.

Do not redesign unrelated parts of the app or change existing functionality. Most importantly, preserve the current Spotify API optimization architecture so these changes do not cause additional unnecessary API calls or bring back the previous rate-limit problem.
