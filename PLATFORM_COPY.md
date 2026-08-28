# Platform listing copy (Spotify / Apple Music)

The **streaming-platform** title + description for each playlist. This is a *separate layer*
from the in-app copy in `sleep_playlists.json`:

| Layer | Where it shows | Style | Source of truth |
|---|---|---|---|
| **In-app** `title` / `subtitle` | LuxRise sleep-sound tile, and the homepage marquee on luxrise.io | 1–3 words, one canonical search phrase | `sleep_playlists.json` |
| **Platform** title + description | Spotify & Apple Music playlist pages | Long-tail, keyword-dense, ≤300 chars | this file |

Editing this file changes nothing automatically — it is the **written record** of what is
pasted into the Spotify / Apple Music playlist editors. Update it whenever you change the
copy on either platform, so the wording survives outside a web form.

## House style

**Title:** `<Name> Sleep - <Descriptor> for <Query 1> & <Query 2>`
The `<Descriptor>` mirrors the in-app subtitle in title case ("Calm Music", "Smooth Jazz").
The two trailing queries are the high-volume phrases that playlist should rank for; "Deep
Sleep" carries most of them.

**Description:** 3 sentences, ~250–300 characters (Spotify's cap is 300).
1. Action verb + the genre term + what it is for.
2. The textures/instruments, then the use cases (bedtime, meditation, studying, naps).
3. The benefit claim — quiet the mind, ease anxiety, restorative rest.

Front-load the genre term; do not open with the brand. Avoid emoji (they render
inconsistently across Apple Music clients and eat the character budget).

---

## Live copy

### Relaxing
**Relaxing Sleep - Calm Music for Deep Sleep & Stress Relief**

Unwind with relaxing sleep music to calm your mind and body. Soft piano, gentle ambient soundscapes, and soothing melodies perfect for falling asleep, meditation, or stress relief. Calming music helps lower anxiety and prepare your body for deep, restorative sleep. Sweet dreams await.

### Healing
**Healing Sleep - Calming Frequencies for Deep Sleep & Meditation**

Relax with healing frequency music designed for deep sleep, meditation, and stress relief. Soothing soundscapes and calming frequencies help quiet the mind, promote relaxation, and create a peaceful atmosphere for restful sleep, mindfulness, and emotional well-being.

### Baby
**Baby Sleep - Soothing Lullabies for Babies & Toddlers**

Help your baby fall asleep faster with calming lullabies, gentle melodies, and peaceful sleep music. Perfect for bedtime routines, naps, and creating a relaxing environment for babies and toddlers. Soft sounds designed to encourage deeper, longer, and more restful sleep.

### Jazz
**Jazz Sleep - Smooth Jazz for Deep Sleep & Relaxation**

Relax and drift into deep sleep with smooth jazz, soft piano, mellow saxophone, and calming late-night vibes. Perfect for bedtime, relaxation, reading, or reducing stress. Gentle jazz melodies create a peaceful atmosphere that helps quiet the mind and support restful sleep.

### Lofi
**Lofi Sleep - Chill Beats for Deep Sleep & Relaxation**

Drift off to peaceful Lofi hip hop beats perfect for sleeping, studying, or unwinding. Mellow Lofi music and chill instrumentals designed to help you fall asleep faster and sleep deeper. Ideal for bedtime routines, meditation, or creating a calm atmosphere. Updated with the best Lofi sleep music

### Rain
**Rain Sleep - Natural Rain Sounds for Deep Sleep & Relaxation**

Fall asleep to soothing rain sounds with gentle rainfall, thunderstorms, and rain on windows. Perfect for insomnia relief, stress reduction, and peaceful sleep. Natural recordings help mask disruptive noises and promote deep, restorative rest. Ideal for meditation, relaxation, or studying

### White Noise
**White Noise Sleep - Pure White Noise for Better Sleep**

Experience uninterrupted sleep with pure white noise designed to mask unwanted sounds. Features white, pink, and brown noise variations proven to help with insomnia and focus. Perfect for light sleepers, babies, and noise-sensitive individuals. Creates consistent sound for deeper, longer sleep.

### Piano
**Piano Sleep - Peaceful Piano Music for Deep Sleep & Relaxation**

Fall asleep to gentle piano melodies crafted for deep sleep, relaxation, and stress relief. Soft instrumental piano music creates a calming atmosphere perfect for bedtime, meditation, reading, or unwinding after a long day. Peaceful sounds to help you sleep deeply and wake refreshed.

### Handpan
**Handpan Sleep - Healing Hang Drum Music for Deep Sleep & Meditation**

Drift off to healing handpan music with warm, resonant hang drum tones. Meditative melodies perfect for deep sleep, yoga, mindfulness, and stress relief. Soft percussive harmonics help quiet a racing mind, ease anxiety, and guide your body into calm, restorative rest.

### Video Game
**Video Game Sleep - Calm Game Soundtracks for Deep Sleep & Relaxation**

Fall asleep to calm video game music and ambient game soundtracks. Nostalgic OST melodies, peaceful overworld themes, and gentle arrangements slowed down for bedtime. Perfect for sleeping, studying, or late-night relaxation with the games you grew up with.
