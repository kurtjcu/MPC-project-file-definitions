# 06 — Audio Program

> **XPJ Format** | [Index](README.md) | **Manual:** [MPC Feature Reference](A1-mpc-feature-reference.md)

Audio tracks (type 6) handle recorded or imported audio. The `program.audio` sub-object is minimal — the actual audio content lives in type-4 events within sequence clips.

## Schema

```json
{
  "program": {
    "type": 6,
    "audio": {
      "inputSource": 0,
      "monitorState": 0
    }
  }
}
```

## Fields

| Field | Type | Description |
| ----- | ---- | ----------- |
| `inputSource` | int | Audio input assignment (0 = default) |
| `monitorState` | int | Input monitoring mode (0 = off) |

## Audio Content: Type-4 Events

Audio clips are stored as type-4 events in the sequence clip for the audio track. Each event carries a complete audio reference:

```json
{
  "version": 2,
  "time": 0,
  "type": 4,
  "channel": 0,
  "audio": {
    "version": 1,
    "sample": {
      "version": 1,
      "name": "02 Music",
      "path": "02 Music.wav",
      "loadImpl": 0,
      "metadata": {
        "tempo": 130.875,
        "rootNote": 60,
        "key": "E Major"
      }
    },
    "name": "02 Music",
    "tempo": 91.99999237060547,
    "volume": 0.5011872053146362,
    "mute": false,
    "reverse": false,
    "fadeIn": 0,
    "fadeOut": 0,
    "repeats": 0,
    "sliceStart": 0,
    "sliceEnd": 4544217,
    "warpEnable": false,
    "userSelectableWarpPoolIndex": 14,
    "warpMarkers": [],
    "coarseTune": 0,
    "fineTune": 0,
    "length": 151680
  }
}
```

## Audio Event Fields

| Field | Type | Description |
| ----- | ---- | ----------- |
| `sample` | object | Embedded sample reference (name, path, metadata) |
| `name` | string | Display name |
| `tempo` | float | Playback tempo for time-stretching |
| `volume` | float | Clip volume (linear scale) |
| `mute` | bool | Clip mute state |
| `reverse` | bool | Reverse playback |
| `fadeIn` / `fadeOut` | int | Fade duration in pulses |
| `repeats` | int | Number of repeats (0 = no repeat) |
| `sliceStart` / `sliceEnd` | int | Audio region in sample frames |
| `warpEnable` | bool | Time-warp enabled |
| `warpMarkers` | array | Warp marker positions (empty when warp disabled) |
| `coarseTune` / `fineTune` | int | Pitch adjustment |
| `length` | int | Event length in pulses |

## Notes

- Only 3 audio events found across all 17 files — all from the Capsun audio demo project.
- Audio events embed their sample reference directly (unlike note events which reference the track's sample pool).
- `sample.metadata.tempo` is the original file tempo; `audio.tempo` is the playback tempo for time-stretching.
- The `volume` value `0.5011872053146362` corresponds to approximately -6dB (linear).

Full examples: [track-audio.json](examples/track-audio.json) | [event-audio.json](examples/event-audio.json)
