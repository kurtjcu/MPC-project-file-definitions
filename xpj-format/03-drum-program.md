# 03 — Drum Program

> **XPJ Format** | [Index](README.md) | **Manual:** [MPC Feature Reference](A1-mpc-feature-reference.md)

The drum program (type 0) is the most complex track type. It provides 128 pad-instrument slots, each with up to 8 sample layers, a complete synthesizer section, per-pad mixer, and pad effects.

## Schema

```json
{
  "program": {
    "version": 4,
    "name": "HipHop Kit",
    "type": 0,
    "programPads": { /* ... */ },
    "mixable": { /* track-level mixer — see 07-mixer-routing.md */ },
    "drum": {
      "version": 2,
      "drumVersion": 12,
      "padNoteMap": { /* 128-entry indexed-dict: {noteForPad: {value0..value127}} */ },
      "instruments": [ /* 128 instrument slots */ ],
      "playbackProperties": { /* modifier slot metadata */ },
      "modifierPlaybackProperties": [ /* 16 entries: slot-to-name map */ ],
      "coarseTune": 0,
      "fineTune": 0,
      "pitch": 0.0,
      "monophonic": false,
      "poliphony": 6,
      "portamentoTime": 0.0,
      "portamentoLegato": false,
      "portamentoQuantised": false,
      "monoRetrigger": false,
      "driftSpeed": 0.0,
      "freeRunningLfoData": { /* value0, value1 */ },
      "padGroup": { /* value0..value127 */ }
    }
  }
}
```

## Drum Program Top-Level Fields

All paths relative to `tracks[n].program.drum`:

| Field | Type | Description |
| ----- | ---- | ----------- |
| `version` | int | Drum sub-format version; always `2` |
| `drumVersion` | int | Additional drum format version; always `12` |
| `instruments[]` | array | 128 pad/instrument slots (see Instrument section below) |
| `padNoteMap` | object | 128-entry indexed-dict `{noteForPad: {value0..value127}}` — MIDI note per pad |
| `playbackProperties` | object | Modifier slot metadata |
| `modifierPlaybackProperties[]` | array | 16-entry slot-to-name map for note modifier system — see [13-note-modifiers.md](13-note-modifiers.md) |
| `coarseTune` | int | Global coarse tune in semitones (-36 to +36) |
| `fineTune` | int | Global fine tune in cents (-99 to +99) |
| `pitch` | float | Global pitch offset (floating-point, separate from coarseTune) |
| `monophonic` | bool | Global monophonic mode |
| `poliphony` | int | Global polyphony voice count (note: schema typo, missing 'y') |
| `portamentoTime` | float | Glide time in seconds; `0.0` = no portamento |
| `portamentoLegato` | bool | Portamento only in legato; always `false` |
| `portamentoQuantised` | bool | Portamento pitch quantized to semitones; always `false` |
| `monoRetrigger` | bool | Mono mode note retrigger behavior; always `false` |
| `driftSpeed` | float | Free-running LFO drift speed; `0.0` = no drift |
| `freeRunningLfoData` | object | Two free-running LFO parameter sets: `{value0, value1}` |
| `padGroup` | object | 128-entry indexed-dict for pad grouping: `{value0..value127}` |

## Instrument Slot Structure

Each of the 128 `instruments[]` entries represents one pad. All paths relative to `tracks[n].program.drum.instruments[i]`:

```json
{
  "version": 27,
  "coarseTune": 0,
  "fineTune": 0,
  "monophonic": false,
  "polyphony": 6,
  "triggerMode": 0,
  "whichMuteGroup": 0,
  "muteTargets": [],
  "simultPlayTargets": [],
  "velocityScale": 100,
  "layerCrossfade": 0,
  "layerCrossfadeX": 0.5,
  "layerCrossfadeY": 0.5,
  "lowNote": 0,
  "highNote": 127,
  "bpmLock": false,
  "tempo": 120.0,
  "warpEnable": false,
  "stretchPercentage": 100,
  "userSelectableWarpPoolIndex": 14,
  "midiOutBehaviour": 1,
  "midiOutChannel": 0,
  "midiOutNote": 0,
  "editAllLayers": false,
  "ignoreBaseNote": false,
  "keyTrackEnable": false,
  "partialPresetName": "<none>",
  "randomPlaySeed": 12345,
  "zonePlayTime": 1,
  "articulationUseXY": false,
  "articulations": { /* value0..value3 */ },
  "chopProperties": { /* version, chopMode, chopThreshold, ... */ },
  "modLinks": [ /* 32 modulation routing slots */ ],
  "noteCounters": { /* note on/off tracking */ },
  "layersv": [ /* 8 layer slots — see Layer Fields */ ],
  "synthSection": { /* per-pad synthesis — see Synth Section */ },
  "mixable": { /* per-pad mixer — see 07-mixer-routing.md */ },
  "padEffects": { /* DrumFX processing chain (8 slots per pad) */ }
}
```

### Instrument (Pad) Fields

| Field | Type | Description |
| ----- | ---- | ----------- |
| `version` | int | Instrument format version; always `27` |
| `coarseTune` | int | Per-pad coarse tune (-36 to +36) |
| `fineTune` | int | Per-pad fine tune (-99 to +99 cents) |
| `monophonic` | bool | Per-pad monophonic mode |
| `polyphony` | int | Per-pad voice count |
| `triggerMode` | int | Sample play mode: `0`=One Shot, `1`=Note Off, `2`=Note On |
| `whichMuteGroup` | int | Mute group assignment (0=none, 1-32=group) |
| `muteTargets` | array | Pads to mute when this pad plays (up to 4 indices) |
| `simultPlayTargets` | array | Pads to trigger simultaneously (up to 4 indices) |
| `velocityScale` | int | Per-pad velocity scale (50-200) |
| `layerCrossfade` | int | Layer crossfade mode |
| `layerCrossfadeX` | float | XY crossfade X position |
| `layerCrossfadeY` | float | XY crossfade Y position |
| `lowNote` | int | Lower MIDI note boundary; `0` = no limit |
| `highNote` | int | Upper MIDI note boundary; `127` = no limit |
| `bpmLock` | bool | Lock sample BPM to project tempo |
| `tempo` | float | Sample BPM (for warp/lock) |
| `warpEnable` | bool | Time-stretch/warp enabled |
| `stretchPercentage` | int | Stretch amount as percentage; `100` = no stretch |
| `userSelectableWarpPoolIndex` | int | Warp algorithm preset index; `14` default |
| `midiOutBehaviour` | int | MIDI output behavior; `1` = as-played |
| `midiOutChannel` | int | MIDI output channel for pad; `0` = default |
| `midiOutNote` | int | MIDI output note for pad; `0` = assigned note |
| `editAllLayers` | bool | Edit all 8 layers simultaneously; always `false` |
| `ignoreBaseNote` | bool | Ignore base note for pitch; always `false` |
| `keyTrackEnable` | bool | Pitch tracks MIDI note (`false` for drums) |
| `partialPresetName` | string | Preset loaded into pad; `"<none>"` = none |
| `randomPlaySeed` | int | Random seed for layer randomization (varies per pad) |
| `zonePlayTime` | int | Zone play time mode; `1` |
| `articulationUseXY` | bool | Use XY pad for articulation; always `false` |
| `articulations` | object | 4 articulation slots: `{value0..value3}` each with `articulationType`, `articulationSpeed`, etc. |
| `chopProperties` | object | Chop sample settings: `{version, chopMode, chopThreshold, chopMinSliceTime, ...}` |
| `modLinks` | array | Mod Matrix — 32 modulation routing slots |
| `noteCounters` | object | Note on/off voice tracking counters |
| `layersv[]` | array | Sample layers — see Layer Fields section |
| `synthSection` | object | Per-pad synthesis section — see Synth Section |
| `mixable` | object | Per-pad mixer — see [07-mixer-routing.md](07-mixer-routing.md) |
| `padEffects` | object | DrumFX processing chain (8 slots per pad) |

## Layer Fields

Each `instruments[i].layersv[]` has 8 layer slots (most empty in practice).

> **Layer volume correction:** Layer volume is a simple float (0.0-1.0), NOT an object. Track-level `program.mixable.volume` is also a float. The `{gainCoefficient, controlValue, law}` volume object appears ONLY at the track-strip level in some contexts — not in `layersv[]`.

```json
{
  "version": 3,
  "active": true,
  "sampleName": "Kick-Tape MPC-Tpe1Q1N3.wav",
  "sampleStart": 0,
  "sampleEnd": 138996,
  "loopStart": 0,
  "loopEnd": 138996,
  "loop": false,
  "direction": 0,
  "sliceIndex": 0,
  "offset": 0,
  "pitch": 0.0,
  "volume": 1.0,
  "pan": 0.5,
  "velocityStart": 0.0,
  "velocityEnd": 1.0,
  "keyTrackEnable": false,
  "rootNote": 60,
  "loopCrossfadeLength": 0,
  "loopFineTune": 0.0,
  "layerLoopModeOverridesSliceLoopMode": false,
  "playbackOffset": 0.0,
  "quadrantEnabled": false,
  "sliceCycleLength": 0,
  "sliceIncrement": 0,
  "sliceIncrementRngSeed": 0,
  "sliceInfo": { /* slice marker data: positions within sample */ },
  "pitchRandom": 0.0,
  "VolumeRandom": 0.0,
  "PanRandom": 0.0,
  "OffsetRandom": 0.0
}
```

| Field | Type | Description |
| ----- | ---- | ----------- |
| `version` | int | Layer format version |
| `active` | bool | Whether this layer slot has a sample loaded |
| `sampleName` | string | Filename linking to track sample pool |
| `sampleStart` | int | Sample start point in sample frames |
| `sampleEnd` | int | Sample end point in sample frames |
| `loopStart` | int | Loop start point in sample frames |
| `loopEnd` | int | Loop end point in sample frames |
| `loop` | bool | Loop enabled |
| `direction` | int | Playback direction: `0`=forward, `1`=reverse |
| `sliceIndex` | int | Slice number for sliced samples |
| `offset` | int | Playback offset |
| `pitch` | float | Pitch offset in semitones |
| `volume` | float | Layer volume (0.0-1.0 normalized) — simple float, not an object |
| `pan` | float | Layer pan (0.0-1.0, 0.5=center) |
| `velocityStart` | float | Velocity range start (0.0-1.0) |
| `velocityEnd` | float | Velocity range end (0.0-1.0) |
| `keyTrackEnable` | bool | Pitch follows MIDI note (`false` for drums, `true` for keygroups) |
| `rootNote` | int | MIDI root note for pitch reference |
| `loopCrossfadeLength` | int | Loop crossfade in samples; `0`=no crossfade |
| `loopFineTune` | float | Fine tune at loop point for seamless loops |
| `layerLoopModeOverridesSliceLoopMode` | bool | Layer loop overrides slice loop mode |
| `playbackOffset` | float | Additional start offset |
| `quadrantEnabled` | bool | Pad quadrant mode active for layer |
| `sliceCycleLength` | int | Slice cycle length for cycling playback |
| `sliceIncrement` | int | Slice increment step |
| `sliceIncrementRngSeed` | int | Random seed for slice increment randomization |
| `sliceInfo` | object | Slice marker data: positions within sample |
| `pitchRandom` | float | Randomized pitch variation per hit |
| `VolumeRandom` | float | Randomized volume variation per hit (note: PascalCase) |
| `PanRandom` | float | Randomized pan variation per hit (note: PascalCase) |
| `OffsetRandom` | float | Randomized start offset per hit (note: PascalCase) |

## Synth Section

All paths relative to `...instruments[i].synthSection`:

The synth section features a **dual filter** architecture (`filterData.value0` for Filter A, `filterData.value1` for Filter B) with blend and serial/parallel routing, four ADSR envelopes, two LFOs, and velocity/randomization modifiers.

```json
{
  "version": 1,
  "filterData": {
    "value0": {
      "filterCutoff": 1.0,
      "filterResonance": 0.0,
      "filterType": 0,
      "filterEnvelopeAmount": 0.0,
      "filterKeytrack": 0.0,
      "filterVelocity": 0.0,
      "filterEnvelopeVelocity": 0.0,
      "outputLevel": 1.0,
      "afterTouchToFilter": 0.0,
      "cutoffRandom": 0.0
    },
    "value1": { /* same fields for Filter B */ }
  },
  "filterBlend": 0.0,
  "filterSerialRouting": false,
  "ampEnvelope": {
    "Attack": { "value0": 0.0 },
    "Decay": { "value0": 0.16808 },
    "Sustain": { "value0": 1.0 },
    "Release": { "value0": 0.01485 }
  },
  "filterEnvelope": { /* same ADSR structure */ },
  "pitchEnvelope": { /* same ADSR structure */ },
  "auxEnvelope": { /* same ADSR structure */ },
  "pitchEnvelopeAmount": 0.0,
  "auxEnvelopeAmount": 0.0,
  "lfoData": { /* LFO 1 + LFO 2 parameters */ },
  "velocitySensitivity": 0.0,
  "velocityToPan": 0.0,
  "velocityToPitch": 0.0,
  "velocityToStart": 0.0,
  "attackRandom": 0.0,
  "decayRandom": 0.0,
  "randomisationScale": 0.0,
  "driftSpeed": 0.0,
  "rampData": { /* ramp generator parameters */ }
}
```

### Top-Level Synth Fields

| Field | Type | Description |
| ----- | ---- | ----------- |
| `version` | int | SynthSection format version |
| `filterData` | object | Dual filter: `{value0: {Filter A}, value1: {Filter B}}` |
| `filterBlend` | float | Blend between filter A and B (0.0=A only, 1.0=B only) |
| `filterSerialRouting` | bool | `true`=serial (A then B), `false`=parallel |
| `ampEnvelope` | ADSR | Amplitude envelope |
| `filterEnvelope` | ADSR | Filter envelope (controls filter cutoff) |
| `pitchEnvelope` | ADSR | Pitch envelope |
| `auxEnvelope` | ADSR | Auxiliary envelope (generic mod source) |
| `pitchEnvelopeAmount` | float | Pitch envelope depth |
| `auxEnvelopeAmount` | float | Aux envelope depth |
| `lfoData` | object | LFO 1 + LFO 2 parameters (two LFOs per pad) |
| `velocitySensitivity` | float | Velocity to overall pad volume |
| `velocityToPan` | float | Velocity to pan position |
| `velocityToPitch` | float | Velocity to pitch |
| `velocityToStart` | float | Velocity to sample start point |
| `attackRandom` | float | Random variation on envelope attack |
| `decayRandom` | float | Random variation on envelope decay |
| `randomisationScale` | float | Global scale for all random parameters |
| `driftSpeed` | float | Pad-level drift speed (separate from drum-level) |
| `rampData` | object | Ramp generator parameters |

### Per-Filter Fields

Each filter (`filterData.value0` and `filterData.value1`) contains:

| Field | Type | Description |
| ----- | ---- | ----------- |
| `filterCutoff` | float | Filter cutoff (0.0-1.0) |
| `filterResonance` | float | Filter resonance (0.0-1.0) |
| `filterType` | int | Filter model (see filter types table below) |
| `filterEnvelopeAmount` | float | How much filter envelope moves cutoff |
| `filterKeytrack` | float | Filter keytracking amount; `0.0` = none |
| `filterVelocity` | float | Velocity to filter cutoff |
| `filterEnvelopeVelocity` | float | Velocity to filter envelope amount |
| `outputLevel` | float | Post-filter output level |
| `afterTouchToFilter` | float | Aftertouch to filter cutoff |
| `cutoffRandom` | float | Random variation on cutoff per hit |

**Envelope quirk:** ADSR values are wrapped as `{"value0": 0.123}` — not plain floats. Keys are Pascal-case (`Attack`, not `attack`).

### Filter Types Observed

| `filterType` | Count in Corpus | Likely Mapping |
| :---: | :---: | --- |
| 0 | 5,039 | Low-pass |
| 1 | 93 | Band-pass |
| 2 | 708 | High-pass |
| 3-13 | sparse | Various (Bypass, other models) |

## Notes

- 5,888 total instrument slots scanned; 40 have non-default envelope settings.
- `padNoteMap` is a 128-entry indexed-dict mapping pad index to MIDI note numbers.
- `modifierPlaybackProperties[]` defines the 16 modifier slot names — see [13-note-modifiers.md](13-note-modifiers.md).
- The `modLinks` array provides 32 modulation routing slots per instrument (replaces the old `modulationMatrix` description).
- The randomization fields (`pitchRandom`, `VolumeRandom`, `PanRandom`, `OffsetRandom`) use inconsistent casing — `pitchRandom` is camelCase while the others are PascalCase. Preserve this when writing.

Full example: [instrument-drum.json](examples/instrument-drum.json)
