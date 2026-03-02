# 10 — Event Types

> **XPJ Format** | [Index](README.md) | **Manual:** [MPC Feature Reference](A1-mpc-feature-reference.md)

Events are stored in `trackClipMaps[row][col].eventList.events[]`. There are 4 event types, each with a shared header and a type-specific payload.

## Shared Event Header

Every event has these fields:

| Field | Type | Description |
| ----- | ---- | ----------- |
| `version` | int | Always 2 |
| `time` | int | Position in pulses from clip start |
| `type` | int | 1=track automation, 2=pad automation, 3=note, 4=audio clip |
| `channel` | int | MIDI channel (always 0) |
| `selected` | bool | UI selection state |
| `muted` | bool | Event mute state |
| `invented` | bool | System-generated flag (always false) |

**Important:** Type 0 has never been observed in any file.

## Type 1 — Track-Level Automation

Controls track-level parameters. The `note` field is always 256 (sentinel for "track-level, not pad-specific").

```json
{
  "version": 2,
  "time": 0,
  "type": 1,
  "channel": 0,
  "selected": false,
  "muted": false,
  "invented": false,
  "automation": {
    "note": 0,
    "value": 0.0,
    "parameter": 131
  }
}
```

See [11-automation.md](11-automation.md) for parameter IDs.

## Type 2 — Pad-Level Automation

Controls per-pad parameters. The `note` field contains the MIDI note number of the target pad.

```json
{
  "version": 2,
  "time": 0,
  "type": 2,
  "channel": 0,
  "selected": false,
  "muted": false,
  "invented": false,
  "automation": {
    "note": 256,
    "value": 0.17777778208255768,
    "parameter": 4104
  }
}
```

### Automation Sub-Object

| Field | Type | Description |
| ----- | ---- | ----------- |
| `note` | int | 256 = track-level sentinel; 0–127 = pad MIDI note |
| `value` | float | Parameter value (0.0–1.0) |
| `parameter` | int | Parameter ID (see [11-automation.md](11-automation.md)) |

## Type 3 — Note Event

MIDI note events for drum/keygroup/plugin tracks. Contains the full note with 16 modifier slots.

```json
{
  "version": 2,
  "time": 0,
  "type": 3,
  "channel": 0,
  "selected": false,
  "muted": false,
  "invented": false,
  "note": {
    "version": 1,
    "note": 39,
    "velocity": 0.8425197005271912,
    "length": 377,
    "probability": 100,
    "ratchet": 1,
    "articulation": 197,
    "modifierValue0": 0.5,
    "modifierActiveState0": true,
    "modifierValue1": 0.5,
    "modifierActiveState1": false,
    // ... modifierValue2–15, modifierActiveState2–15
    "EnumCerealisationWrapper(selectedModifierType)": "Tuning (coarse)"
  }
}
```

### Note Sub-Object

| Field | Type | Description |
| ----- | ---- | ----------- |
| `note` | int | MIDI note number (0–127) |
| `velocity` | float | 0.0–1.0 (normalized from 0–127) |
| `length` | int | Duration in pulses |
| `probability` | int | 0–100, chance of playing |
| `ratchet` | int | Ratchet count (1 = normal, 2+ = retrigs) |
| `articulation` | int | Articulation value (always 197 in data) |
| `modifierValue0`–`15` | float | Modifier slot values |
| `modifierActiveState0`–`15` | bool | Whether each modifier is enabled |
| `EnumCerealisationWrapper(selectedModifierType)` | string | UI: last-selected modifier |

See [13-note-modifiers.md](13-note-modifiers.md) for the modifier slot mapping.

### Note Statistics

- 8,052 total note events across 17 files
- MIDI note range: 3–90
- Velocity range: 0.0–1.0
- Fixed values: `articulation=197`, `probability=100`, `ratchet=1` (all notes)

## Type 4 — Audio Clip Event

Audio clip placement for audio tracks (type 6). See [06-audio-program.md](06-audio-program.md) for the full `audio` sub-object schema.

```json
{
  "version": 2,
  "time": 0,
  "type": 4,
  "channel": 0,
  "selected": false,
  "muted": false,
  "invented": false,
  "audio": {
    "version": 1,
    "sample": { /* embedded sample ref */ },
    "name": "02 Music",
    "tempo": 91.99999237060547,
    "volume": 0.5011872053146362,
    // ... warp, slice, tune fields
    "length": 151680
  }
}
```

Only 3 type-4 events found across all files (all from Capsun demo).

## Event Count Summary

| Type | Name | Count | Files |
| ---- | ---- | ----- | ----- |
| 1 | Track automation | 9,552 | 11 |
| 2 | Pad automation | 375 | 2 |
| 3 | Note | 8,052 | 17 |
| 4 | Audio clip | 3 | 1 |

## Notes

- Type 1 and 2 events can coexist in the same clip — automation events are interleaved with note events, sorted by `time`.
- The `time` field is always relative to the clip start (not the sequence start).
- `selected`, `muted`, and `invented` are always false in saved files — they reflect transient UI/playback state.

Full examples: [event-note.json](examples/event-note.json) | [event-automation.json](examples/event-automation.json) | [event-audio.json](examples/event-audio.json)
