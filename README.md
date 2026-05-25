# Jingles

Jingles are short audio clips that play when you hover over a game on your grid. Think of the 3DS home menu or the PS Vita, where each game has its own little sound. Cocoon’s jingle system works the same way, with options for per-game clips, per-platform fallbacks, and a global default. ([Cocoon Shell][1])

---

## Getting Started

Head to Settings > Library & Data > Jingles to set everything up. This is where you manage your jingle repositories, tweak playback settings, and set your fallback jingles.

If you just want something playing for every game without fussing over individual assignments, set a Global Default jingle and you’re done. Every game that doesn’t have its own jingle will use that one. ([Cocoon Shell][1])

---

## How Jingles Play

When you land on a game tile, Cocoon waits about 600ms before playing anything. This prevents jingles from firing while you’re scrolling through your library quickly.

Once the delay passes, Cocoon looks for a jingle in this order:

1. Per-game jingle — set specifically for that game.
2. Per-platform default — a fallback for all games on that platform.
3. Global default — the catch-all fallback.

If nothing is set at any level, no jingle plays.

Background music ducks automatically while a jingle is playing, then fades back in when it finishes. ([Cocoon Shell][1])

---

# Jingle Repositories

Repositories are GitHub repos that host collections of jingles organized by platform and game. Instead of tracking down audio files yourself, you can add a community repo and pull jingles from it directly. ([Cocoon Shell][1])

## Adding a Repository

1. Go to Settings > Library & Data > Jingles.
2. Under Repositories, tap **Add New Repository**.
3. Enter the repo in `owner/repo` format.
4. The repo now shows up as a source in the jingle picker.

You can add as many repos as you want. Remove one by pressing the X button next to it. ([Cocoon Shell][1])

---

## How Matching Works

Repos contain an `index.json` that maps game names to audio files.

Matching priority:

* Dedicated `regex` field
* `name` as regex fallback
* Bidirectional substring matching

Examples:

* `"Mario Kart"` matches `"Super Mario Kart"`
* Invalid regex falls back silently to substring matching
* Regex patterns longer than 200 characters are skipped

Matching entries are automatically sorted to the top of the picker. ([Cocoon Shell][1])

---

# Making Your Own Repository

Your repository needs an `index.json` in the root.

## Flat Structure

```json
{
  "name": "My Jingle Pack",
  "entries": [
    {
      "name": "Super Mario Bros",
      "file": "jingles/smb.mp3"
    },
    {
      "name": "Zelda - A Link to the Past",
      "file": "jingles/zelda.mp3",
      "regex": "zelda.*(link|alttp|past)"
    }
  ]
}
```

## Platform-Organized Structure

```json
{
  "name": "My Jingle Pack",
  "n3ds": [
    {
      "name": "Animal Crossing",
      "file": "jingles/n3ds/animal-crossing.mp3"
    }
  ],
  "snes": [
    {
      "name": "Super Metroid",
      "file": "jingles/snes/metroid.ogg"
    }
  ]
}
```

Platform keys use Cocoon’s internal IDs like:

* `n3ds`
* `snes`
* `psx`
* `gba`

Audio files are fetched using GitHub raw URLs. Cocoon checks the `main` branch first, then `master`. ([Cocoon Shell][1])

---

# Entry Fields

| Field   | Required | Description                      |
| ------- | -------- | -------------------------------- |
| `name`  | Yes      | Display name shown in the picker |
| `file`  | Yes      | Relative path to the audio file  |
| `regex` | No       | Regex used for game matching     |

Legacy repos can also use `game` as an alias for `name`. ([Cocoon Shell][1])

---

# Using the `regex` Field

Example:

```json
{
  "name": "Zelda - A Link to the Past",
  "file": "zelda_alttp.ogg",
  "regex": "zelda.*(link|alttp|past)"
}
```

More examples:

```json
{
  "name": "Fire Emblem",
  "file": "fire-emblem.mp3",
  "regex": "fire.?emblem"
}
```

```json
{
  "name": "Mario Kart",
  "file": "mario-kart.mp3",
  "regex": "mario.kart.*(ds|wii|7|8)"
}
```

```json
{
  "name": "Pokémon",
  "file": "pokemon.mp3",
  "regex": "pok[eé]mon"
}
```

```json
{
  "name": "Super Mario Bros",
  "file": "smb.mp3",
  "regex": "^(super )?mario bros\\.?$"
}
```

### Matching Rules

1. `regex` field is checked first.
2. If missing, `name` is treated as regex.
3. Matching is partial by default.
4. Invalid regex falls back to substring matching.
5. Regex over 200 characters is treated as plain text. ([Cocoon Shell][1])

---

# Tips for Repo Authors

* Use `regex` for flexible matching.
* Keep regex under 200 characters.
* Invalid regex is okay — fallback matching still works.
* Use `^` and `$` for exact matching.
* Escape regex characters with `\\` in JSON.
* Organize large collections by platform.
* MP3 and OGG are the most common formats. ([Cocoon Shell][1])

---

# Assigning Jingles to Games

## From the Jingle Picker

Open a game’s properties and select the jingle option.

### Controls

| Button | Action              |
| ------ | ------------------- |
| X      | Preview play/pause  |
| A      | Download and assign |
| Y      | Edit search term    |
| B      | Go back             |

Each highlighted entry shows animated action buttons. ([Cocoon Shell][1])

---

## Uploading a Local File

Select **Upload File** from the left pane of the picker to import your own audio file.

The file is copied into Cocoon’s media folder and assigned automatically. ([Cocoon Shell][1])

---

# Fallback Jingles

Fallbacks let you avoid assigning every game manually.

## Global Default

Any game without a per-game or per-platform jingle uses the global default. ([Cocoon Shell][1])

## Per-Platform Defaults

Examples:

* SNES → SNES startup sound
* PS1 → PlayStation boot sound
* GBA → Game Boy jingle

Navigate to:

`Settings > Library & Data > Jingles > Per-Platform Defaults`

Controls:

* `A` → Set jingle
* `X` → Clear jingle

Per-platform defaults override the global default but are overridden by per-game jingles. ([Cocoon Shell][1])

---

# Playback Settings

## Auto Fade Out

Long jingles fade out smoothly after a configurable duration.

* Adjustable from 3–30 seconds
* Default: 10 seconds
* Uses a 30-step fade ramp

Enabled by default. ([Cocoon Shell][1])

---

## Volume Normalization

Cocoon analyzes audio and adjusts playback gain so jingles stay at a consistent loudness.

* Loud clips are reduced
* Quiet clips are boosted

Enabled by default. ([Cocoon Shell][1])

---

## Volume

Jingles use their own dedicated volume slider.

Default: `30%`

Path:

`Settings > Sounds > Jingle Volume`

This value is also included in theme exports. ([Cocoon Shell][1])

---

# Background Music Interaction

When a jingle starts:

* Background music fades down over ~300ms

When it ends:

* Background music fades back in over ~800ms

On dual-screen setups, jingles route to the main display audio output. ([Cocoon Shell][1])

---

# Cleanup

The **Clear All Jingles** option:

* Removes all per-game assignments
* Deletes downloaded jingle files
* Keeps fallback settings intact

Missing files are skipped silently without showing errors. ([Cocoon Shell][1])

---

# Supported Formats

* MP3
* OGG
* WAV
* FLAC
* AAC
* M4A

Any Android-supported audio format generally works. ([Cocoon Shell][1])

[1]: https://cocoon-shell.com/wiki/jingles/?utm_source=chatgpt.com "Jingles | Cocoon Shell"
