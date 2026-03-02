# 02 — Track Types

> **XPJ Format** | [Index](README.md) | **Manual:** [MPC Feature Reference](A1-mpc-feature-reference.md)

Every track in `data.tracks[]` shares a common skeleton, with a `program.type` field determining which sub-objects are present.

## Program Type Table

| Type | Name | Count | Sub-objects | Notes |
| ---- | ---- | ----- | ----------- | ----- |
| 0 | Drum | 48 | `program.drum` | Standard drum/sample program |
| 1 | Keygroup | 18 | `program.drum` + `program.keygroup` | Chromatic multi-zone sampler |
| 2 | _(unknown)_ | 0 | — | Never observed |
| 3 | Plugin | 1 | `program.plugin` | Software instrument (Bassline, etc.) |
| 4 | MIDI | 29 | _(none)_ | MIDI output track — no sampler/synth engine |
| 5 | CV | 3 | `program.cv` | CV/Gate output for modular gear |
| 6 | Audio | 15 | `program.audio` | Audio recording/playback track |
| 7 | Return | 72 | _(none)_ | Effect return bus (4 per project) |
| 8 | Submix | 144 | _(none)_ | Submix bus (8 per project) |
| 9 | Output | 288 | _(none)_ | Hardware output pair (up to 16 per project) |
| 10 | Input | — | _(none)_ | Hardware input (in `mixer`, not `tracks`) |

## Shared Track Skeleton

Every track object (all types) has these fields:

```json
{
  "version": 5,
  "name": "HipHop Kit",
  "volume": 1.0,
  "volumeKnown": true,
  "pan": 0.5039370059967041,
  "panKnown": true,
  "mute": false,
  "cvPort": 0,
  "gatePort": 1,
  "velocityScale": 1.0,
  "muteGroup": 0,
  "transposition": 0,
  "colour": 43775,
  "padsFollowTrackColour": false,
  "skipFromRowLaunch": false,
  "samples": [ /* sample pool for this track */ ],
  "program": {
    "version": 4,
    "name": "HipHop Kit",
    "type": 0,
    "programPads": { /* pad colour/assignment */ },
    "mixable": { /* track-level mixer */ },
    // ... type-specific sub-objects
  },
  "outputPort": { "type": 1, "deviceName": "<none>" },
  "outputChannel": 0,
  "midiMonitorable": { "state": 2 }
}
```

## Track-Level Field Reference

All paths relative to `tracks[n]`:

| Field | Type | Description |
| ----- | ---- | ----------- |
| `version` | int | Track schema version (always 5) |
| `name` | string | Display name |
| `volume` | float | Track fader level (0.0–1.0, NOT the `mixable.volume`) |
| `volumeKnown` | bool | Whether volume was explicitly set (false = default) |
| `pan` | float | Track pan (0.0–1.0, 0.5=center). Default `0.5039...` |
| `panKnown` | bool | Whether pan was explicitly set (false = default) |
| `mute` | bool | Track mute state |
| `colour` | int | Track colour as packed RGB integer |
| `padsFollowTrackColour` | bool | Whether pads inherit track color; always `false` |
| `muteGroup` | int | Track-level mute group (0 = none) |
| `transposition` | int | Track-level transposition in semitones |
| `velocityScale` | float | Velocity scaling factor; `1.0` = 100% |
| `cvPort` | int | CV output port assignment; `0` = default |
| `gatePort` | int | Gate output port assignment; `1` = first port |
| `length` | int | Track length in pulses; `0` = follows sequence |
| `lengthFollowsSequenceLength` | bool | Track length follows sequence; always `true` |
| `skipFromRowLaunch` | bool | Skip in Clip Matrix row launch; always `false` |
| `recordArm` | bool | Record arm state |
| `samples[]` | array | Track-level sample pool (see [12-samples.md](12-samples.md)) |
| `arrangementClipMap` | array | Arrangement view clip assignments; always `[]` |
| `sharedClipMap` | array | Clip Matrix shared clip map; always `[]` |
| `midiEventsFilter` | object | MIDI filter (channel, velocity, note, automation) |
| `midiMonitorable` | object | MIDI monitor mode: `{state: "Off"\|"In"\|"Auto"\|"Merge"}` |
| `midiBankAndProgramNumber` | object | MIDI Bank Select + Program Change |
| `midiInputRoute` | object | MIDI input routing: `{inputPort: {deviceName}, inputChannel}` |
| `midiOutputRoute` | object | MIDI output routing: `{outputPort: {deviceName}, outputChannel}` |
| `outputPort` | object | Hardware output port: `{type: int, deviceName: string}` |
| `outputChannel` | int | MIDI output channel |
| `program` | object | Program object — type-specific sub-objects below |

## Shared Program-Level Fields

All paths relative to `tracks[n].program` (present on ALL program types):

| Field | Type | Description |
| ----- | ---- | ----------- |
| `version` | int | Program schema version; always `4` |
| `name` | string | Program display name (kit/instrument name) |
| `type` | int | Program type — see dispatch table above |
| `transpose` | int | Program-level transpose in semitones; `0` |
| `mixable` | object | Track-level mixer (see [07-mixer-routing.md](07-mixer-routing.md)) |
| `programPads` | object | Pad config: `{PadsFollowTrackColour, Type, Universal, pads, ...}` |
| `midiEventsFilter` | object | MIDI filter at program level (parallel to track-level) |
| `midiKillGroup` | int | Program-level MIDI kill group; `-1` = none |
| `chainID` | int | Chain identifier; `0` = no chain |
| `renderable` | object | Cue bus routing: `{sendToCueBus: bool}` |
| `xfaderRoute` | int | X-Fader routing; `0` = default |
| `customQLinks` | array | 16 custom Q-Link assignments for this program |
| `fxRackQLinks` | array | FX Rack Q-Link assignments; always `[]` |
| `customMacroSceneData` | object | Custom macro scene data (8 scene slots) |
| `fxRackMacroSceneData` | object | FX Rack macro scene data (8 scene slots) |
| `customisable` | object | Controller mapping customization config |
| `base.*` | objects | Controller assignment sub-objects (see [14-unknown-fields.md](14-unknown-fields.md)) |

## Type 4 — MIDI Tracks

MIDI tracks route note and CC data to external gear. No `drum`/`keygroup`/`plugin`/`audio`/`cv` sub-object — the program carries routing and filter configuration instead.

**Direct fields (10):** `chainID`, `customMacroSceneData`, `customQLinks`, `customisable`, `fxRackMacroSceneData`, `fxRackQLinks`, `midiEventsFilter`, `midiKillGroup`, `renderable`, `transpose`

**Controller assignments (~40):** `base.customXFaders`, `base.customXYPad1`–`4`, `base.customPadGrid`, `base.customEnvFollower`, `base.customPhysicalPadsAssignments` (each with MacroSceneData + fxRack variants)

### MIDI Events Filter

```json
"midiEventsFilter": {
  "channelFilterData": { "channel": 255 },
  "velocityFilterData": { "min": 0.0, "max": 1.0 },
  "noteFilterData": { "noteRange": { "type": 2, "data": "111...128 chars..." } },
  "automationFilterData": { "filteredParams": "00000000000000000" }
}
```

- `channel: 255` = all channels pass through
- `noteRange.data` = 128-char bitmask (1 = note passes, 0 = filtered)
- `filteredParams` = 17-char bitmask for automation parameter filtering

## Type 5 — CV Tracks

CV tracks output pitch/gate/modulation signals for modular synthesizers. The `program.cv` sub-object has two modes controlled by `isDrumProgram`.

```json
"cv": {
  "isDrumProgram": false,
  "modulationPort": -1,
  "velocityPort": -1,
  "noteTracking": "Last",
  "parameters": [ /* 255 float values */ ],
  "drumMappingData": {
    "version": 1,
    "padIndexToCvControl": [
      [{ "CvPort": 0, "DataType": "Gate" }],
      [{ "CvPort": 1, "DataType": "Gate" }]
      // ...
    ]
  }
}
```

### CV Modes

| Field | Melodic (`isDrumProgram: false`) | Drum (`isDrumProgram: true`) |
| ----- | ------------------------------- | ---------------------------- |
| Behavior | Pitch CV + Gate from note data | Individual pads → CV ports |
| `padIndexToCvControl` | 8 entries | 86 entries |
| `DataType` values | `"Gate"` | `"Gate"`, `"Note"` |

### CV Fields

| Field | Type | Description |
| ----- | ---- | ----------- |
| `isDrumProgram` | bool | false=Melodic, true=Drum mode |
| `modulationPort` | int | CV port for mod wheel (-1 = none) |
| `velocityPort` | int | CV port for velocity (-1 = none) |
| `noteTracking` | string | Note priority: `"Last"` observed |
| `parameters` | float[255] | CV parameter values (envelopes, LFOs, etc.) |
| `drumMappingData.padIndexToCvControl` | array | Per-pad CV port and data type assignments |

## Infrastructure Tracks (Types 7/8/9)

These tracks carry no musical content. They provide routing and effects infrastructure:

- **Return (7):** 4 per project — `Return 1` through `Return 4`. Receives from send buses.
- **Submix (8):** 8 per project — `Submix 1` through `Submix 8`. Track grouping.
- **Output (9):** Up to 16 per project — `Out 1/2` through `Out 31/32` (stereo pairs). Hardware outputs.

All infrastructure tracks have `volumeKnown: false`, `panKnown: false` (defaults), and empty `samples[]`.

## Notes

- Keygroup tracks (type 1) have BOTH `program.drum` AND `program.keygroup`. The `drum.instruments[]` array provides the per-zone infrastructure; `keygroup` adds zone mapping metadata.
- Type 4 (MIDI) has no program-specific sub-object but carries `midiEventsFilter` for channel/note/velocity filtering.
- Type 5 (CV) confirmed with Melodic and Drum modes — matching the manual description exactly.
- Track count per project: 30–39 total (varies by content track count; infrastructure is fixed at 28).
- Type 2 remains unobserved across all 18 project files.

Full examples: [track-drum.json](examples/track-drum.json) | [track-keygroup.json](examples/track-keygroup.json) | [track-plugin.json](examples/track-plugin.json) | [track-midi.json](examples/track-midi.json) | [track-cv-melodic.json](examples/track-cv-melodic.json) | [track-cv-drum.json](examples/track-cv-drum.json) | [track-audio.json](examples/track-audio.json) | [track-return.json](examples/track-return.json)
