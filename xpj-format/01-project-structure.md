# 01 — Project Structure

> **XPJ Format** | [Index](README.md) | **Reference:** [Format Reference](00-format-reference.md)

The top-level `data{}` object contains the entire project state. Across 18 analyzed files, it has ~66 keys: 4 vary between projects, the rest are constant or quasi-constant.

## Schema

```json
{
  "formatVersion": 2,
  "data": {
    "version": 28,
    "key": "C Major",
    "masterTempo": 120.0,
    "masterTempoEnabled": false,
    "engineMode": 5,
    "currentSequence": 0,
    "currentTrackIndex": 0,
    "emulation": 0,
    "lastSavedProductIdentifier": "ACVB",
    "originalCreatorProductIdentifier": "ACVB",
    "tracks": [ /* ... track objects */ ],
    "sequences": [ /* ... {key, value} wrapped */ ],
    "songs": [ /* ... 32 empty song objects */ ],
    "samples": [ /* ... sample pool */ ],
    "mixer": { /* ... infrastructure track mirrors */ },
    "qlinkMode": "Screen",
    "value0": "1 Bar",
    // ... 50+ constant fields
  }
}
```

## Varying Fields

These 4 fields change between projects — everything else is constant:

| Field | Type | Values Observed | Notes |
| ----- | ---- | --------------- | ----- |
| `currentTrackIndex` | int | 0, 1, 2 | UI state: last-selected track |
| `engineMode` | string | `"Main Mode"`, `"Grid View"`, `"Arrange"` | UI state: active view when saved |
| `masterTempo` | float | 120.0, 128.0 | Global tempo (overrides sequence BPM when enabled) |
| `qlinkMode` | string | `"ClipGridLevel"`, `"Screen"` | UI state: Q-Link knob mode |

## Project Metadata

| Field | Type | Value | Notes |
| ----- | ---- | ----- | ----- |
| `version` | int | 28 | Schema version for firmware 3.7.x |
| `key` | string | `"C Major"` | Project key signature |
| `masterTempoEnabled` | bool | false | When true, `masterTempo` overrides per-sequence BPM |
| `emulation` | int | 0 | Hardware emulation target (0 = none) |
| `lastSavedProductIdentifier` | string | `"ACVB"` | Product code of hardware that last saved |
| `originalCreatorProductIdentifier` | string | `"ACVB"` | Product code of hardware that created project |

## Track/Sequence Containers

| Field | Type | Description | See Section |
| ----- | ---- | ----------- | ----------- |
| `tracks[]` | array | All tracks (content + infrastructure) | [02-track-types.md](02-track-types.md) |
| `sequences[]` | array | Sequence slots, `{key, value}` wrapped | [09-sequences-events.md](09-sequences-events.md) |
| `songs[]` | array | 32 song mode slots (always empty in corpus) | [14-unknown-fields.md](14-unknown-fields.md) |
| `samples[]` | array | Project-level sample pool | [12-samples.md](12-samples.md) |
| `mixer` | object | Redundant mirror of infrastructure tracks | [07-mixer-routing.md](07-mixer-routing.md) |
| `currentSequence` | int | Active sequence index; always `0` | — |

## Global Settings

All constant across 18 analyzed files:

| Field | Type | Value | Description |
| ----- | ---- | ----- | ----------- |
| `trackMutePerSequence` | bool | true | Track mute state is per-sequence |
| `quantiser` | object | `{enabled, timeDivision, swingLevel}` | Quantization config |
| `arpeggiatorProperties` | object | `{enabled, action, latch, arpIndex, ...}` | Arpeggiator state |
| `padPerformSettings` | object | `{version, startOctave, chord, velocity, ...}` | Pad Perform mode settings |
| `midiNoteFilterPipe` | object | `{"EnumCerealisationWrapper(behaviour)": "Off"}` | Global MIDI note filter |
| `midiSendDestinations` | object | `{destinations: []}` | MIDI output routing targets |
| `modifierPlaybackProperties` | object | 16-entry array | Per-parameter modifier mode (Relative/Absolute) |
| `stepSequencerBehaviour` | object | — | Step Sequencer mode settings |
| `savedRecordDestination` | int | 1 | Last recording destination (1 = first user track) |
| `padQuadrantEnablement` | bool | false | Pad quadrant split mode |
| `parameterSnapshotterData` | object | `{snapshotGroupData, morphingGroupData, ...}` | Snapshot system state |
| `xyfxResponder` | object | `{index: int}` | XYFX slot index |
| `locators` | object | `{version, names, positions, colours}` | Arrangement locators |
| `clipPlayerData` | object | `{version, trackClipTransportMap}` | Clip Matrix transport state |
| `sharedClipMatrixData` | array | `[]` | Shared clip matrix state (always empty) |

## UI State Fields

All constant across corpus:

| Field | Type | Value | Description |
| ----- | ---- | ----- | ----------- |
| `currentClipRow` | int | 0 | Clip Matrix selected row |
| `currentPadGridMode` | int | 1 | Pad Grid mode |
| `currentXFaderMode` | int | 1 | X-Fader mode |
| `currentXYPad1Mode`–`4Mode` | int | 1 | XY Pad modes (4 separate fields) |
| `currentAssignableXFader` | int | 0 | Selected Assignable X-Fader slot |
| `currentAssignablePadBank` | int | 0 | Selected Assignable Pad Bank |
| `currentAssignableEnvelopeFollower` | int | 0 | Selected Assignable Envelope Follower |
| `currentEnvFollowerMode` | int | 1 | Envelope Follower mode |
| `currentMIDITriggeredTrackMuteType` | int | 0 | MIDI-triggered mute type |
| `assignablePhysicalPadsMode` | int | 3 | Assignable physical pad mode |
| `value0` | string | `"1 Bar"` | Default bar unit |
| `tuiMainModeLayout` | object | — | Hardware screen Main Mode layout |
| `tuiMainModeMixerLayout` | object | — | Hardware screen Mixer layout |
| `horizontalClipMatrixView` | object | — | Clip Matrix scroll position (horizontal) |
| `mpcClipMatrixView` | object | — | Clip Matrix scroll position (MPC-style) |

## Controller Mapping Fields

All constant across corpus:

| Field | Type | Description |
| ----- | ---- | ----------- |
| `qlinkProjectModeAssignments` | object | Q-Link knob assignments in Project mode |
| `qlinkProjectModeAssignments2` | object | Secondary Q-Link project assignments |
| `qlinkPadParamModeAssignments` | object | Q-Link assignments in Pad Parameter mode |
| `qlinkPadSceneModeAssignments` | object | Q-Link assignments in Pad Scene mode |
| `midiLearnSettings` | object | MIDI Learn bindings: `{controls: []}` |
| `assignableEnvelopeFollowerAssignments` | object | Envelope Follower controller assignments |
| `assignablePadGridAssignments` | object | Pad Grid controller assignments |
| `assignablePhysicalPadsData` | object | Physical pad button assignments: `{first, second}` |
| `assignableXFaderAssignments` | object | X-Fader controller assignments |
| `assignableXYPadAssignments1`–`4` | object | XY Pad controller assignments (4 XY pads) |
| `rowLaunchSnapshotAssignments` | object | Row Launch controller assignments |
| `scene` | object | MIDI scene controller map: `{mapping: list[8], value: ?}` |
| `mpcControlSurfaceBehaviour` | object | Hardware control surface behavior settings |

## Notes

- `sequences[]` uses the `{key, value}` wrapper pattern. `songs[]` does NOT — it's a plain array.
- `masterTempo` is only active when `masterTempoEnabled: true` (always false in our data — each sequence carries its own BPM).
- `ACVB` = MPC Standalone product identifier. Desktop MPC software uses different identifiers.
- BPM values often appear as imprecise floats (e.g., `93.00003814697266`) due to 32-bit float storage — see [00-format-reference.md](00-format-reference.md) §Known Quirks.
