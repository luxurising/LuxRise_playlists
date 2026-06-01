# LuxRise Playlists

Remote content for the **LuxRise** iOS app. Editing the JSON here changes the app's
sleep-sound catalog **without an app update or App Store review** — every install picks
up changes on its next cold launch.

The app fetches:

```
https://raw.githubusercontent.com/luxurising/LuxRise_playlists/refs/heads/main/sleep_playlists.json
```

It caches the result and falls back to a built-in copy if the fetch fails, so a bad push
won't break offline/first-launch users — but it **will** ship the next time they're online.
Validate before you push.

---

## `sleep_playlists.json`

A JSON **array** of playlist objects. Order in the file doesn't matter — `sortOrder` controls
display order.

```json
[
  {
    "id": "lofiSleep",
    "title": "Lofi Sleep",
    "subtitle": "Lofi beats",
    "colorName": "purple",
    "icon": "moon.stars.fill",
    "spotifyId": "spotify:playlist:53YSODrxL1tsZFFLf3fym4",
    "appleMusicId": "pl.u-PDb40JgTeDbdJYG",
    "youtubeId": "PLzIl52FhfQpx28YCeXr20Ix1mmT7O62U0",
    "coverArtUrl": null,
    "sortOrder": 0
  }
]
```

### Fields

| Field | Required | Notes |
|---|---|---|
| `id` | **Yes** | **Stable slug — never change it once shipped.** It's the value the app persists when a user selects this sound, and the artwork-cache key. Renaming it orphans those. Lowercase, no spaces (e.g. `oceanWaves`). |
| `title` | **Yes** | Display name. Safe to rename anytime (identity is `id`, not title). |
| `subtitle` | No | Short descriptor under the title (e.g. `"Nature sounds"`). Defaults to empty. |
| `colorName` | **Yes** | Gradient fallback shown before artwork loads. One of: `blue`, `gray`, `indigo`, `green`, `purple`, `orange`, `red`, `teal`. Unknown → blue. |
| `icon` | **Yes** | An **SF Symbol** name (e.g. `moon.stars.fill`, `cloud.rain.fill`, `waveform`). Browse names in Apple's *SF Symbols* app. |
| `spotifyId` | **Yes** | Full Spotify URI: `spotify:playlist:<id>`. |
| `appleMusicId` | **Yes** | Apple Music playlist ID (starts with `pl.`). |
| `youtubeId` | No | YouTube playlist ID. Currently unused by the app; safe to omit. |
| `coverArtUrl` | No | Override artwork URL. Usually omit (`null`) — the app fetches real artwork from the connected service and caches it. |
| `sortOrder` | No | Ascending integer for picker order (0 first). Omit → sorts last. Keep them unique. |

> The **Silence** option is built into the app — don't add it here.

---

## How to add a playlist

1. **Create the playlist** on Spotify *and* Apple Music (both are needed — the app plays
   whichever service the user is connected to).
2. **Get the Spotify URI:** in Spotify, Share → Copy link, e.g.
   `https://open.spotify.com/playlist/53YSODrxL1tsZFFLf3fym4` → use
   `spotify:playlist:53YSODrxL1tsZFFLf3fym4`.
3. **Get the Apple Music ID:** in Music, Share → Copy link, e.g.
   `https://music.apple.com/.../playlist/lofi-sleep/pl.u-PDb40JgTeDbdJYG` → use the
   trailing `pl.u-PDb40JgTeDbdJYG`.
4. **Pick a unique `id`** (stable slug), a `colorName`, an `icon` (SF Symbol), and a
   `sortOrder`.
5. **Add the object** to the array.
6. **Validate**, then commit & push to `main`:
   ```bash
   python3 -m json.tool sleep_playlists.json   # fails loudly on malformed JSON
   git add sleep_playlists.json && git commit -m "content: add <name>" && git push
   ```

Live on every install's next cold launch.

## To remove / rename

- **Remove:** delete the object. Any user who had it selected falls back to **Silence**.
- **Rename display name:** change `title` only — leave `id` alone.
- **Reorder:** change `sortOrder` values.
