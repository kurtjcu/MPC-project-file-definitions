# 09 — Sequences & Events

> **XPJ Format** | [Index](README.md) | **Manual:** [MPC Feature Reference](A1-mpc-feature-reference.md)

Sequences are the primary musical containers. Each sequence holds tempo, time signature, and a clip matrix mapping tracks to event lists.

## Sequence Access Pattern

Sequences use the `{key, value}` wrapper pattern:

```json
{
  "sequences": [
    {
      "key": 0,
      "value": {
        "version": 5,
        "name": "HipHop Template",
        "bpm": 93.00003814697266,
        "lengthBars": 16,
        "loopStartBar": 0,
        "loopEndBar": 16,
        "loop": true,
        "tempoEnable": true,
        "transposition": 0,
        "timeSignatureTrack": {
          "timeSignatures": [
            { "beatsPerBar": 4, "beatLength": 960, "barStart": 0 }
          ]
        },
        "trackClipMaps": [ /* clip matrix */ ],
        "seqEventList": { /* always empty */ },
        "locators": { /* 6 named markers */ },
        "lengthPulses": 61440,
        "loopStartPulses": 0,
        "loopEndPulses": 61440,
        "autoSelectTrackIndex": -1
      }
    }
  ]
}
```

## Sequence Fields

| Field | Type | Description |
| ----- | ---- | ----------- |
| `name` | string | Display name |
| `bpm` | float | Sequence tempo (used when `tempoEnable: true`) |
| `lengthBars` | int | Sequence length in bars |
| `lengthPulses` | int | Sequence length in pulses (= `lengthBars * beatsPerBar * beatLength`) |
| `loopStartBar` / `loopEndBar` | int | Loop region in bars |
| `loopStartPulses` / `loopEndPulses` | int | Loop region in pulses |
| `loop` | bool | Loop enabled (always true in our data) |
| `tempoEnable` | bool | Use sequence BPM (always true) |
| `transposition` | int | Sequence-level transposition |
| `autoSelectTrackIndex` | int | -1 = no auto-select |

## Time Signatures

```json
"timeSignatureTrack": {
  "timeSignatures": [
    { "beatsPerBar": 4, "beatLength": 960, "barStart": 0 }
  ]
}
```

| Field | Type | Description |
| ----- | ---- | ----------- |
| `beatsPerBar` | int | Numerator (e.g., 4) |
| `beatLength` | int | Denominator in pulses: 960=quarter, 480=eighth, 1920=half |
| `barStart` | int | Bar where this time sig takes effect |

Only 4/4 observed across all files. Multiple time signatures per sequence are structurally supported.

**Time signatures are global** — the MPC manual confirms that time signature changes apply to all sequences, even though each sequence has its own `timeSignatureTrack` in JSON.

### beatLength Values for Common Time Signatures

| Time Signature | `beatsPerBar` | `beatLength` | Pulses per Bar |
| -------------- | :-----------: | :----------: | :------------: |
| 4/4 | 4 | 960 | 3,840 |
| 3/4 | 3 | 960 | 2,880 |
| 6/8 | 6 | 480 | 2,880 |
| 7/8 | 7 | 480 | 3,360 |
| 5/4 | 5 | 960 | 4,800 |
| 2/4 | 2 | 960 | 1,920 |

### 960 PPQ Conversion Table

| Note Value | Pulses | Notes |
| ---------- | :----: | ----- |
| Whole note | 3,840 | 4 beats |
| Half note | 1,920 | 2 beats |
| Quarter note | 960 | 1 beat (base unit) |
| 8th note | 480 | |
| 16th note | 240 | |
| 32nd note | 120 | |
| Quarter triplet | 640 | 960 * 2/3 |
| 8th triplet | 320 | 480 * 2/3 |
| 16th triplet | 160 | 240 * 2/3 |
| Dotted quarter | 1,440 | 960 * 1.5 |
| Dotted 8th | 720 | 480 * 1.5 |

## Clip Matrix: trackClipMaps

The clip matrix is a doubly-nested array of `{key, value}` entries:

```json
"trackClipMaps": [
  [
    {
      "key": "Audio 001",
      "value": {
        "version": 2,
        "launchQuantisation": 1,
        "startPulses": 0,
        "endPulses": 61440,
        "loopStartPulses": 0,
        "loopEndPulses": 61440,
        "loop": true,
        "legato": true,
        "launch": 0,
        "name": "Audio 001",
        "colour": 0,
        "eventList": {
          "length": 9223372036854775807,
          "events": [ /* note, automation, or audio events */ ],
          "version": 2,
          "quantisation": {
            "version": 1,
            "pulses": 0,
            "swing": 0.0,
            "strength": 1.0
          },
          "numFilterTypes": 30
        },
        "perClipParameterValues": { /* per-clip overrides */ }
      }
    }
  ]
]
```

Structure: `trackClipMaps[row][col]` where each entry maps a track name to a clip.

## Clip Fields

| Field | Type | Description |
| ----- | ---- | ----------- |
| `launchQuantisation` | int | Launch quantize setting |
| `startPulses` / `endPulses` | int | Clip boundaries |
| `loopStartPulses` / `loopEndPulses` | int | Clip loop region |
| `loop` | bool | Clip loops |
| `legato` | bool | Legato mode |
| `launch` | int | Launch mode (0 = trigger) |
| `name` | string | Clip display name |
| `colour` | int | Clip colour (0 = default) |
| `eventList.events[]` | array | The actual musical content |
| `eventList.quantisation` | object | Input quantize settings |

## Event List Quantisation

```json
{
  "pulses": 0,
  "swing": 0.0,
  "strength": 1.0
}
```

- `pulses`: Quantize grid (0 = off, 240 = 16th notes, 480 = 8th notes, 960 = quarter notes)
- `swing`: Swing amount (0.0–1.0)
- `strength`: Quantize strength (0.0–1.0, 1.0 = full quantize)

## seqEventList

The `seqEventList` at sequence level is always empty across all files. All events live in `trackClipMaps[].eventList.events[]`.

## Locators

Each sequence has 6 named markers:

```json
"locators": {
  "version": 1,
  "names": ["1", "2", "3", "4", "5", "6"],
  "positions": [
    { "bar": 0, "beat": 0, "pulse": 0 }
    // ... 6 entries
  ],
  "colours": [16711867, 16711935, 16746496, 16711799, 16711867, 43775]
}
```

## Songs

`data.songs[]` is a plain array (NOT `{key, value}` wrapped — unlike sequences). All 544 song slots across 17 files are empty.

```json
"songs": [
  { "items": [], "name": "Song 1" },
  // ... 32 entries total
]
```

## Notes

- All 18 files contain exactly 1 sequence each.
- BPM range observed: 91.99–175.00.
- Bar lengths: 2–48 bars.
- `eventList.length` is always `9223372036854775807` (int64 max) — appears to be a sentinel.
- Pulse math: 16 bars in 4/4 = `16 * 4 * 960 = 61440` pulses.

Full example: [sequence.json](examples/sequence.json)
