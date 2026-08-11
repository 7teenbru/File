Please audit and fix my Spotify API architecture.

This is an iPod-style Spotify client, and I want it to remain within Spotify’s normal API limits without requiring Extended Quota Mode.

First, inspect the existing code and determine:

* Which Spotify endpoints are being called
* How frequently they’re being called
* Which calls are duplicated or unnecessary
* Whether any endpoint is being polled too frequently
* Where the app is failing to cache data

Then implement the following:

1. Build a centralized Spotify API manager

Every Spotify API request should go through one networking/request manager.

It should:

* Track requests
* Prevent duplicate simultaneous requests
* Queue requests when necessary
* Prioritize requests needed for the currently visible UI
* Cancel requests that are no longer needed

2. Add persistent caching

Cache Spotify data locally, including:

* Tracks
* Albums
* Artists
* Playlists
* User library data
* Album artwork URLs/data where appropriate

When data is already cached:

* Display the cached version immediately
* Don’t make another request simply because the user navigated away and came back
* Refresh stale data intelligently in the background

Use appropriate expiration/staleness rules rather than refreshing everything constantly.

3. Fix playback polling

This is especially important.

Do NOT request Spotify’s playback state every second.

Instead:

* Fetch the current playback state when necessary
* Maintain the displayed playback position locally between synchronizations
* Periodically synchronize only when needed
* Synchronize when the app returns to the foreground
* Synchronize after user actions that could change playback
* Stop unnecessary polling while the app is backgrounded

The UI should still show a smooth, continuously updating progress bar without continuously contacting Spotify.

4. Use lazy loading

Only request information when the user actually needs it.

For example:

* Don’t download an entire library when the app opens
* Don’t load every playlist’s tracks until the user opens that playlist
* Don’t request album information repeatedly when navigating back to an already-loaded album

Use pagination correctly.

5. Handle HTTP 429 correctly

If Spotify returns HTTP 429:

* Read the Retry-After header
* Stop sending requests during that period
* Queue appropriate requests
* Retry after the specified delay
* Use exponential backoff if repeated 429 responses occur

Do NOT try to bypass Spotify’s rate limits.

6. Make the UI cache-first

The app should feel fast even when Spotify is slow.

Preferred flow:

User opens screen
→ show cached data immediately if available
→ refresh only if necessary
→ update UI when fresh data arrives

Do not make the user wait for a Spotify API request when we already have usable cached data.

7. Add request diagnostics

In development builds, add logging for:

* Spotify endpoint
* timestamp
* cache hit/miss
* HTTP status
* request duration
* 429 responses
* Retry-After value
* number of API requests during the current session

If practical, add a small developer-only screen showing API request statistics.

8. Important constraints

Do NOT:

* Use undocumented Spotify endpoints
* Circumvent Spotify’s rate limits
* Create multiple Spotify apps/accounts to evade limits
* Continuously poll Spotify
* Make unnecessary API calls simply to keep cached data “fresh”

Use Spotify’s documented API behavior and current developer requirements.

Before making changes, inspect the existing project and explain briefly what is currently causing excessive API usage. Then implement the fixes directly in the project.

Do not rewrite working parts of the app unnecessarily. Preserve the existing iPod-style UI and functionality.
