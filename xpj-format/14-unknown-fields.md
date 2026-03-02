# 14 — Unknown & Constant Fields

> **XPJ Format** | [Index](README.md)

This section provides the comprehensive field-by-field catalog for fields present in the XPJ JSON that are not documented in the MPC 3.7 manual, or that are constant across all 18 analyzed files. Downstream Pydantic model authors need this to know what fields to include in the schema.

> **Summary:** ~120 undocumented fields across 7 nesting levels. All constant values are safe to default-fill. Fields marked UNKNOWN require hardware testing to characterize.

## 14.1 Summary by Nesting Level

| Nesting Level | Total Keys | Undocumented / Constant |
|---------------|:----------:|:-----------------------:|
| Top-level (`data.{}`) | 66 | 43 undocumented + 11 potentially documented |
| Track-level (`data.tracks[].{}`) | 27 | 20 undocumented |
| Program-level (`data.tracks[].program.{}`) | 46 | 40 undocumented |
| Drum program (`...program.drum.{}`) | 15 | 10 undocumented |
| Drum instrument/pad (`...drum.instruments[].{}`) | 37 | 24 undocumented |
| Effect container (`...inserts.effects[].{}`) | 3 | 1 undocumented |
| Effect plugin (`...inserts.effects[].plugin.{}`) | 7 | 4 undocumented |
| **Total** | — | **~120** |

## 14.2 Top-Level Undocumented Fields

### UI State

Transient fields that reflect hardware state when saved, not musical content.

| Path | Type | Observed Values | Description | Confidence | Coverage |
|------|------|-----------------|-------------|:----------:|:--------:|
| `currentTrackIndex` | int | VARIES: 0, 1, 2 | Index of last-selected track | HIGH | CONFIRMED |
| `engineMode` | string | VARIES: `"Grid View"`, `"Main Mode"`, `"Arrange"` | Active view when saved | HIGH | CONFIRMED |
| `qlinkMode` | string | VARIES: `"ClipGridLevel"`, `"Screen"` | Q-Link assignment mode | HIGH | CONFIRMED |
| `currentClipRow` | int | `0` | Selected Clip Matrix row | HIGH | CONSTANT |
| `currentPadGridMode` | int | `1` | Pad Grid display mode | MEDIUM | CONSTANT |
| `currentXFaderMode` | int | `1` | X-Fader mode | MEDIUM | CONSTANT |
| `currentXYPad1Mode`–`4Mode` | int | `1` (each) | XY Pad 1-4 modes | MEDIUM | CONSTANT |
| `currentAssignableXFader` | int | `0` | Selected Assignable X-Fader slot | HIGH | CONSTANT |
| `currentAssignablePadBank` | int | `0` | Selected Assignable Pad Bank | HIGH | CONSTANT |
| `currentAssignableEnvelopeFollower` | int | `0` | Selected Assignable Envelope Follower | HIGH | CONSTANT |
| `currentEnvFollowerMode` | int | `1` | Envelope Follower mode | MEDIUM | CONSTANT |
| `currentMIDITriggeredTrackMuteType` | int | `0` | MIDI-triggered mute type | MEDIUM | CONSTANT |
| `assignablePhysicalPadsMode` | int | `3` | Physical pad button mode | LOW | CONSTANT |
| `savedRecordDestination` | int | `1` | Last recording destination | MEDIUM | CONSTANT |
| `value0` | string | `"1 Bar"` | Default bar unit (context unclear) | LOW | CONSTANT |
| `horizontalClipMatrixView` | object | `{clipMatrixViewAperture}` | Horizontal Clip Matrix scroll position | HIGH | CONSTANT |
| `mpcClipMatrixView` | object | `{clipMatrixViewAperture}` | MPC-style Clip Matrix scroll position | HIGH | CONSTANT |
| `tuiMainModeLayout` | object | `{version, EnumCerealisationWrapper(currentPanel), ...}` | Hardware screen Main Mode layout | HIGH | CONSTANT |
| `tuiMainModeMixerLayout` | object | `{EnumCerealisationWrapper(currentDisplay), ...}` | Hardware screen Mixer layout | HIGH | CONSTANT |

### Controller Mapping

Q-Link, XY Pads, Envelope Followers, X-Fader, and Scene assignments.

| Path | Type | Observed Values | Description | Confidence | Coverage |
|------|------|-----------------|-------------|:----------:|:--------:|
| `qlinkProjectModeAssignments` | object | `{version, controlGroupData, macroSceneData}` | Q-Link assignments in Project mode | HIGH | CONSTANT |
| `qlinkProjectModeAssignments2` | object | `{version, controlGroupData, macroSceneData}` | Secondary Q-Link project assignments | MEDIUM | CONSTANT |
| `qlinkPadParamModeAssignments` | object | `{version, controlGroupData, macroSceneData}` | Q-Link in Pad Parameter mode | HIGH | CONSTANT |
| `qlinkPadSceneModeAssignments` | object | `{version, controlGroupData, macroSceneData}` | Q-Link in Pad Scene mode | HIGH | CONSTANT |
| `assignableEnvelopeFollowerAssignments` | object | `{currentControlIndex, sourceData, controlGroupData}` | Envelope Follower controller assignments | MEDIUM | CONSTANT |
| `assignablePadGridAssignments` | object | `{version, controlGroupData, macroSceneData}` | Pad Grid controller assignments | MEDIUM | CONSTANT |
| `assignablePhysicalPadsData` | object | `{first, second}` | Physical pad button assignments | MEDIUM | CONSTANT |
| `assignableXFaderAssignments` | object | `{version, controlGroupData, macroSceneData}` | X-Fader controller assignments | MEDIUM | CONSTANT |
| `assignableXYPadAssignments1`–`4` | object | `{version, controlGroupData, macroSceneData}` | XY Pad 1-4 controller assignments | MEDIUM | CONSTANT |
| `rowLaunchSnapshotAssignments` | object | `{version, controlGroupData, macroSceneData}` | Row Launch controller assignments | MEDIUM | CONSTANT |
| `midiLearnSettings` | object | `{controls: []}` | MIDI Learn bindings (empty in all corpus files) | MEDIUM | CONSTANT |
| `scene` | object | `{mapping: list[8], value: ?}` | MIDI scene controller mapping (8 scenes, B/Bank/CC/Port format) | LOW | CONSTANT |

### Feature Config

MPC features present but not manually tested.

| Path | Type | Observed Values | Description | Confidence | Coverage |
|------|------|-----------------|-------------|:----------:|:--------:|
| `quantiser` | object | `{enabled: bool, timeDivision: int, swingLevel: float}` | Global quantization settings | HIGH | CONSTANT |
| `arpeggiatorProperties` | object | `{enabled, action, latch, arpIndex, rhythmIndex, patternIndex, velocity, aftertouchAction}` | Arpeggiator settings | HIGH | CONSTANT |
| `padPerformSettings` | object | `{version, startOctave, progressionName, chord, velocity, doAfterTouch, adjustRollWithVelocity, order}` | Pad Perform mode (chord/scale) | HIGH | CONSTANT |
| `stepSequencerBehaviour` | object | `{state: dict}` | Step Sequencer mode config | HIGH | CONSTANT |
| `midiNoteFilterPipe` | object | `{EnumCerealisationWrapper(behaviour): "Off"}` | Global MIDI note filter | MEDIUM | CONSTANT |
| `midiSendDestinations` | object | `{destinations: []}` | MIDI output destinations (empty in corpus) | HIGH | CONSTANT |
| `modifierPlaybackProperties` | object | `{Tuning (coarse): "Relative", ...}` — 16 params | Note Modifier playback mode per slot | HIGH | CONSTANT |
| `parameterSnapshotterData` | object | `{snapshotGroupData, morphingGroupData, ignoreTracksMap: [], ignoreInsertsMap: []}` | Parameter Snapshot system state | HIGH | CONSTANT |
| `xyfxResponder` | object | `{index: int}` | XYFX slot responding to XY pad | HIGH | CONSTANT |
| `locators` | object | `{version, names, positions, colours}` | Arrangement locators (not sequence locators) | HIGH | CONSTANT |
| `clipPlayerData` | object | `{version, trackClipTransportMap}` | Clip Matrix transport state | HIGH | CONSTANT |
| `sharedClipMatrixData` | array | `[]` | Shared Clip Matrix state; always empty | MEDIUM | CONSTANT |
| `padQuadrantEnablement` | bool | `false` | Pad quadrant split mode; always false | HIGH | CONSTANT |
| `mpcControlSurfaceBehaviour` | object | `{version, qlinkTriggerButton, tapTempo, tempoLightFlasher, banksButtons, acv6VuMeters, pads, touchStripResponder, acv7QLinks, padModeProperties}` | Hardware control surface behavior | HIGH | CONSTANT |

### Product Metadata

| Path | Type | Observed Values | Description | Confidence | Coverage |
|------|------|-----------------|-------------|:----------:|:--------:|
| `emulation` | int | `0` | Hardware emulation target; `0` = no emulation | MEDIUM | CONSTANT |
| `lastSavedProductIdentifier` | string | `"ACVB"` | Product code of hardware that last saved (`ACVB` = MPC Standalone) | HIGH | CONSTANT |
| `originalCreatorProductIdentifier` | string | `"ACVB"` | Product code of hardware that created project | HIGH | CONSTANT |

## 14.3 Track-Level Undocumented Fields

All paths relative to `tracks[n]`:

| Path | Type | Observed Values | Description | Confidence | Coverage |
|------|------|-----------------|-------------|:----------:|:--------:|
| `version` | int | `5` | Track schema version (5 = MPC 3.7.x) | HIGH | CONSTANT |
| `volumeKnown` | bool | `false` | Whether volume was explicitly set | MEDIUM | CONSTANT |
| `panKnown` | bool | `false` | Whether pan was explicitly set | MEDIUM | CONSTANT |
| `colour` | int | VARIES (e.g., `16711680`=red) | Track color as packed RGB int | HIGH | CONFIRMED |
| `padsFollowTrackColour` | bool | `false` | Pads inherit track color | HIGH | CONSTANT |
| `muteGroup` | int | `0` | Track-level mute group (0=none) | MEDIUM | CONSTANT |
| `transposition` | int | `0` | Track transposition in semitones | HIGH | CONSTANT |
| `velocityScale` | float | `1.0` | Velocity scaling factor | HIGH | CONSTANT |
| `cvPort` | int | `0` | CV output port; present on all tracks | MEDIUM | CONSTANT |
| `gatePort` | int | `1` | Gate output port; present on all tracks | MEDIUM | CONSTANT |
| `length` | int | `0` | Track length in pulses (0=follows sequence) | HIGH | CONSTANT |
| `lengthFollowsSequenceLength` | bool | `true` | Track follows sequence length | HIGH | CONSTANT |
| `skipFromRowLaunch` | bool | `false` | Skip in Clip Matrix row launch | HIGH | CONSTANT |
| `recordArm` | bool | VARIES | Record arm state | HIGH | CONFIRMED |
| `arrangementClipMap` | array | `[]` | Arrangement view clip assignments | HIGH | CONSTANT |
| `sharedClipMap` | array | `[]` | Clip Matrix shared clip map | MEDIUM | CONSTANT |
| `midiEventsFilter` | object | `{channelFilterData, velocityFilterData, noteFilterData, automationFilterData}` | MIDI filter at track level | HIGH | CONFIRMED |
| `midiMonitorable` | object | `{state: "Off"\|"In"\|"Auto"\|"Merge"}` | MIDI monitor mode | HIGH | CONFIRMED |
| `midiBankAndProgramNumber` | object | `{midiBankEnable, midiBankMsb, midiBankLsb, midiProgramNumberEnable, midiProgramNumber}` | Bank Select + Program Change | HIGH | CONFIRMED |

## 14.4 Program-Level Undocumented Fields (Common to All Types)

All paths relative to `tracks[n].program`:

| Path | Type | Observed Values | Description | Confidence | Coverage |
|------|------|-----------------|-------------|:----------:|:--------:|
| `version` | int | `4` | Program schema version (4 = MPC 3.7.x) | HIGH | CONSTANT |
| `transpose` | int | `0` | Program-level transpose | HIGH | CONSTANT |
| `midiKillGroup` | int | `-1` | Program MIDI kill group (-1=none) | MEDIUM | CONSTANT |
| `chainID` | int | `0` | Program chain identifier | LOW | CONSTANT |
| `renderable` | object | `{sendToCueBus: false}` | Cue bus routing | HIGH | CONSTANT |
| `xfaderRoute` | int | `0` | X-Fader routing | MEDIUM | CONSTANT |
| `programPads` | object | `{PadsFollowTrackColour, Type, Universal, UnusedPads, pads, universalPad}` | Pad configuration | HIGH | CONFIRMED |
| `customQLinks` | array | list(len=16) | 16 custom Q-Link assignments | HIGH | CONSTANT |
| `fxRackQLinks` | array | `[]` | FX Rack Q-Link assignments | HIGH | CONSTANT |
| `customMacroSceneData` | object | `{value0..value7}` | Custom macro scene data (8 slots) | MEDIUM | CONSTANT |
| `fxRackMacroSceneData` | object | `{value0..value7}` | FX Rack macro scene data (8 slots) | MEDIUM | CONSTANT |
| `customisable` | object | `{mapping: dict}` | Controller mapping customization | LOW | CONSTANT |
| `midiEventsFilter` | object | (same structure as track-level) | MIDI filter at program level | HIGH | CONFIRMED |
| `base.customXFaders` | array | `[]` | Per-program custom X-Fader assignments | MEDIUM | CONSTANT |
| `base.customXFaderMacroSceneData` | object | `{value0..value7}` | Macro scene data for X-Fader | MEDIUM | CONSTANT |
| `base.customXYPad1`–`4` | array | `[]` | Per-program custom XY Pad 1-4 | MEDIUM | CONSTANT |
| `base.customXYPad1MacroSceneData`–`4MacroSceneData` | object | `{value0..value7}` | Macro scene data for XY Pads 1-4 | MEDIUM | CONSTANT |
| `base.customPadGrid` | array | `[]` | Per-program custom Pad Grid | MEDIUM | CONSTANT |
| `base.customPadGridMacroSceneData` | object | `{value0..value7}` | Macro scene data for Pad Grid | MEDIUM | CONSTANT |
| `base.customEnvFollower` | array | `[]` | Per-program Envelope Follower | MEDIUM | CONSTANT |
| `base.customEnvFollowerMacroSceneData` | object | `{value0..value7}` | Macro scene data for Env Follower | MEDIUM | CONSTANT |
| `base.customPhysicalPadsAssignments` | array | `[]` | Per-program Physical Pad assignments | MEDIUM | CONSTANT |
| `base.fxRackXFaders` | array | `[]` | FX Rack X-Fader assignments | MEDIUM | CONSTANT |
| `base.fxRackXFaderMacroSceneData` | object | `{value0..value7}` | FX Rack X-Fader macro scene data | MEDIUM | CONSTANT |
| `base.fxRackXYPad1`–`4` | array | `[]` | FX Rack XY Pad 1-4 assignments | MEDIUM | CONSTANT |
| `base.fxRackXYPad1MacroSceneData`–`4MacroSceneData` | object | `{value0..value7}` | FX Rack XY Pad macro scene data | MEDIUM | CONSTANT |
| `base.fxRackPadGrid` | array | `[]` | FX Rack Pad Grid assignments | MEDIUM | CONSTANT |
| `base.fxRackPadGridMacroSceneData` | object | `{value0..value7}` | FX Rack Pad Grid macro scene data | MEDIUM | CONSTANT |
| `base.fxRackEnvFollower` | array | `[]` | FX Rack Envelope Follower | MEDIUM | CONSTANT |
| `base.fxRackEnvFollowerMacroSceneData` | object | `{value0..value7}` | FX Rack Env Follower macro scene data | MEDIUM | CONSTANT |
| `base.fxRackPhysicalPadsAssignments` | array | `[]` | FX Rack Physical Pad assignments | MEDIUM | CONSTANT |

## 14.5 Drum Program Undocumented Fields

All paths relative to `tracks[n].program.drum`:

| Path | Type | Observed Values | Description | Confidence | Coverage |
|------|------|-----------------|-------------|:----------:|:--------:|
| `drumVersion` | int | `12` | Drum sub-format version (12 = MPC 3.7.x) | HIGH | CONSTANT |
| `pitch` | float | `0.0` | Global pitch offset (float, separate from coarseTune int) | MEDIUM | CONSTANT |
| `monoRetrigger` | bool | `false` | Mono mode note retrigger | HIGH | CONSTANT |
| `portamentoTime` | float | `0.0` | Portamento glide time in seconds | HIGH | CONSTANT |
| `portamentoLegato` | bool | `false` | Portamento only in legato | HIGH | CONSTANT |
| `portamentoQuantised` | bool | `false` | Portamento pitch quantized to semitones | HIGH | CONSTANT |
| `driftSpeed` | float | `0.0` | Free-running LFO drift speed | MEDIUM | CONSTANT |
| `freeRunningLfoData` | object | `{value0, value1}` | Two free-running LFO param sets | MEDIUM | CONSTANT |
| `padGroup` | object | `{value0..value127}` (128 entries) | Pad grouping (128 entries, one per pad slot) | MEDIUM | CONSTANT |
| `playbackProperties` | object | Modifier slot metadata | 03-drum-program.md | HIGH | CONFIRMED |

## 14.6 Drum Instrument (Pad) Undocumented Fields

All paths relative to `tracks[n].program.drum.instruments[i]`:

| Path | Type | Observed Values | Description | Confidence | Coverage |
|------|------|-----------------|-------------|:----------:|:--------:|
| `version` | int | `27` | Instrument sub-format version | HIGH | CONSTANT |
| `bpmLock` | bool | VARIES | Sample BPM locks to project tempo | HIGH | CONFIRMED |
| `tempo` | float | VARIES | Detected sample BPM for warp | HIGH | CONFIRMED |
| `warpEnable` | bool | VARIES | Time-stretch/warp enabled | HIGH | CONFIRMED |
| `stretchPercentage` | int | `100` | Stretch amount %; `100`=no stretch | HIGH | CONSTANT |
| `userSelectableWarpPoolIndex` | int | `14` | Warp algorithm preset index | MEDIUM | CONSTANT |
| `lowNote` | int | `0` | Lower MIDI note boundary | HIGH | CONSTANT |
| `highNote` | int | `127` | Upper MIDI note boundary | HIGH | CONSTANT |
| `ignoreBaseNote` | bool | `false` | Ignore base note for pitch | HIGH | CONSTANT |
| `editAllLayers` | bool | `false` | Edit all 8 layers simultaneously | HIGH | CONSTANT |
| `keyTrackEnable` | bool | VARIES | Pitch tracks MIDI note | HIGH | CONFIRMED |
| `layerCrossfadeX` | float | `0.0` | XY crossfade X position | HIGH | CONSTANT |
| `layerCrossfadeY` | float | `0.0` | XY crossfade Y position | HIGH | CONSTANT |
| `midiOutBehaviour` | int | `1` | MIDI output behavior (1=as-played) | MEDIUM | CONSTANT |
| `midiOutChannel` | int | `0` | MIDI output channel for pad | HIGH | CONSTANT |
| `midiOutNote` | int | `0` | MIDI output note for pad | HIGH | CONSTANT |
| `partialPresetName` | string | `"<none>"` | Preset loaded into pad | HIGH | CONSTANT |
| `randomPlaySeed` | int | VARIES (unique) | Random seed for layer randomization | HIGH | CONFIRMED |
| `zonePlayTime` | int | `1` | Zone play time mode | LOW | CONSTANT |
| `articulationUseXY` | bool | `false` | Use XY pad for articulation | HIGH | CONSTANT |
| `articulations` | object | `{value0..value3}` (4 slots) | Articulation definitions per pad | HIGH | CONFIRMED |
| `chopProperties` | object | `{version, chopMode, chopThreshold, chopMinSliceTime, chopRegions, chopBars, chopBeats, chopTimeSignature}` | Chop sample settings | HIGH | CONFIRMED |
| `modLinks` | array | list(len=32) | Mod Matrix: 32 modulation routing slots | HIGH | CONFIRMED |
| `noteCounters` | object | `{value0, value1}` | Note on/off voice tracking counters | LOW | CONSTANT |

## 14.7 Effect Plugin Undocumented Fields

All paths relative to `...inserts.effects[n]`:

| Path | Type | Observed Values | Description | Confidence | Coverage |
|------|------|-----------------|-------------|:----------:|:--------:|
| `index` | int | `0`–`3` | Slot position in insert chain | HIGH | CONFIRMED |

All paths relative to `...inserts.effects[n].plugin`:

| Path | Type | Observed Values | Description | Confidence | Coverage |
|------|------|-----------------|-------------|:----------:|:--------:|
| `version` | int | VARIES | Plugin hosting version | HIGH | CONFIRMED |
| `is64Bits` | bool | `false` | Plugin binary is 64-bit; always false for MPC native | HIGH | CONSTANT |
| `presetName` | string | VARIES (`"Hall Medium"`, `"Default"`) | Loaded preset name | HIGH | CONFIRMED |
| `presetEdited` | bool | `false` | Preset modified from saved; always false | HIGH | CONSTANT |
| `programNumber` | int | VARIES (0–127) | Preset bank selection | HIGH | CONFIRMED |

---

## 14.8 Known Corrections (Errata)

This section lists all known errors in the original `XPJ-Schema-Analysis.md`. The unified schema supersedes the original analysis; these corrections are the authoritative statement of what changed and why.

| # | What XPJ-Schema-Analysis.md States | Correct Value | Source of Correction |
|---|-------------------------------------|---------------|----------------------|
| 1 | `program.type 2=Plugin, 3=MIDI` | Type 3=Plugin (confirmed: 1 file with Bassline Plugin program); type 2=never observed, may not exist in MPC 3.7 | `feature-mapping.md` Program Type Corrections (Phase 2 direct file analysis of all 18 .xpj files) |
| 2 | `event type 0=note event` | Type 0 never observed in any file; type 3=note event, type 1=track automation, type 2=pad automation, type 4=audio clip | `feature-mapping.md` Event Type Corrections (10-event-types.md v1.2 correction) |
| 3 | `data.mixer is authoritative` | `data.mixer` is a redundant mirror of `data.tracks[]` infrastructure tracks; use `tracks[]` as canonical source | `feature-mapping.md` Mixer Object Duplication (Phase 2) |
| 4 | `Layer volume is a simple float` | This is **partially correct** for `layersv[j].volume` (IS a simple float 0.0-1.0); however Phase 2 initially over-corrected: `{gainCoefficient, controlValue, law}` appears at track-strip level, NOT in `layersv[]`. Layer volume confirmed as simple float. | `04-01-SUMMARY.md` Decisions (Phase 4 Plan 01 correction of the over-correction) |
| 5 | `ACVS header is "SerialisableProjectData / json / Linux" on lines 3-5` | Correct header content: Line 1=`ACVS`, Line 2=`1` (header version), Line 3=`28` (schema version matching `data.version`), Line 4=blank, Line 5=blank. JSON body begins at line 6. | `xpj-format/01-project-structure.md` Header (direct line-by-line analysis of 17 .xpj files) |
| 6 | `Automation param 1039=layer level` | Parameter 1039=pad filter cutoff (confirmed from basic-test ExpA file with pad filter automation). Not layer-level. | `xpj-format/11-automation.md` Confirmed Parameters (Phase 2 direct file analysis) |
| 7 | `Known plugin UIDs: 4 items` | 57 confirmed UIDs total: 29 Akai Professional effects (1094xxxxxx range), 25 AIR Music Technology effects (1631xxxxxx range), 1 XYFX (1482xxxxxx), 7 plugin instruments (1114xxxxxx / 1164xxxxxx / 1299xxxxxx / 1329xxxxxx / 1330xxxxxx / 1399xxxxxx / 1415xxxxxx) | `xpj-format/08-effects-catalog.md` + `knowledge/research-effects-plugins.md` (Phase 2 + Phase 4 synthesis) |
