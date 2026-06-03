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
    "title": "Lofi",
    "subtitle": "Lofi beats",
    "colorName": "purple",
    "icon": "moon.stars.fill",
    "spotifyId": "spotify:playlist:53YSODrxL1tsZFFLf3fym4",
    "appleMusicId": "pl.u-PDb40JgTeDbdJYG",
    "youtubeId": "PLzIl52FhfQpx28YCeXr20Ix1mmT7O62U0",
    "coverArtUrl": null,
    "sortOrder": 0,
    "titles":    { "en": "Lofi", "es": "Lofi", "fr": "Lofi", "de": "Lofi", "it": "Lofi", "pt-BR": "Lofi", "ja": "ローファイ", "ko": "로파이", "zh-Hans": "Lofi" },
    "subtitles": { "en": "Lofi beats", "es": "Ritmos Lofi", "fr": "Beats Lofi", "de": "Lofi-Beats", "it": "Ritmi Lofi", "pt-BR": "Beats Lofi", "ja": "Lofiビート", "ko": "로파이 비트", "zh-Hans": "Lofi 节拍" }
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
| `titles` | No | **Localized display names**, keyed by language code (see [Localization](#localization)). Omit → the `title` field is shown to everyone. |
| `subtitles` | No | **Localized descriptors**, keyed by language code. Omit → the `subtitle` field is shown to everyone. |

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
- **Rename display name:** change `title` only — leave `id` alone. If the entry has a `titles` map, update `"en"` there too (and ideally the other languages).
- **Reorder:** change `sortOrder` values.

---

## Localization

Sleep-sound names are translated **here in the JSON** — no app update needed. Add a `titles`
and/or `subtitles` object to an entry, keyed by language code:

```json
"titles":    { "en": "Rain", "es": "Lluvia", "fr": "Pluie", "de": "Regen", "it": "Pioggia", "pt-BR": "Chuva", "ja": "雨音", "ko": "빗소리", "zh-Hans": "雨声" },
"subtitles": { "en": "Nature sounds", "es": "Sonidos de naturaleza", "fr": "Sons de la nature", "de": "Naturgeräusche", "it": "Suoni della natura", "pt-BR": "Natureza", "ja": "自然の音", "ko": "자연의 소리", "zh-Hans": "自然声" }
```

**Supported language codes** (must match exactly): `en`, `es`, `fr`, `de`, `it`, `pt-BR`,
`ja`, `ko`, `zh-Hans`. (`en` matches the top-level `title`/`subtitle` — include it for clarity.)

**How the app resolves a name** (`RemotePlaylist.displayTitle` / `displaySubtitle`):
1. Tries the user's language, region-specific then base (e.g. `pt-BR` → `pt`).
2. Falls back to `en` in the map.
3. Falls back to the top-level `title` / `subtitle`.

So every layer is optional and safe:
- No `titles` map at all → everyone sees the English `title`. (Backward compatible — old
  entries without these keys keep working.)
- A map missing some languages → those users see English. **Partial translation never shows blanks.**

**Keep them SHORT.** The picker tile is narrow — the title is one line, the subtitle a tiny
line under it. Because the whole screen is *already* sleep sounds, the translated titles drop
the redundant "Sleep" word and use just the distinctive term (`Rain` → `Pluie` / `Regen`
/ `雨声`), not a literal "Sommeil…/…-Schlaf". Mirror that style for new sounds. Genre/loanwords
(`Lofi`, `Jazz`, `Ambient`, `Piano`) stay as-is.

> New translations are machine-generated and flagged for native review in the app project —
> have a native speaker sanity-check before relying on them for a store launch.

## Cover art

- Hosted in this repo under `covers/<id>.jpg`, referenced by each entry's `coverArtUrl`
  via a `raw.githubusercontent.com/.../covers/<id>.jpg` URL.
- **Square, ~1024×1024, logo-free.** Keep important content out of the **bottom ~30%** —
  the app overlays the playlist title + subtitle there.
- To swap a cover: replace `covers/<id>.jpg` (same filename) and push — no JSON change.
  Image caching may delay it ~5 min; force it with a clean app reinstall.

## Notes / ideas (not built yet)

- **In-app "Request a playlist".** Idea to let users request sleep playlists from inside
  LuxRise; once a request comes in, adding it here is a quick JSON edit. Design TBD — see
  `plans/sleep-playlists-remote-content.md` in the app repo.
