# 11 — Automation

> **XPJ Format** | [Index](README.md) | **Manual:** [MPC Feature Reference](A1-mpc-feature-reference.md) | Hardware confirmed: 80+ param IDs across all 10 manual categories

Automation is stored as type-1 (track-level) and type-2 (pad-level) events. Phase 10 hardware validation confirmed 80+ unique parameter IDs across all parameter categories, with sweep testing across 14 experiments.

## Confirmed Parameters

> **Hardware Confirmed** — All entries below were hardware-confirmed during Phase 10 sweep testing (2026-02-25 through 2026-03-01). Status column shows sweep test reference.

### Track-Level MIDI CC Automation (params 0–127)

Params 0–127 map 1:1 to standard MIDI CC numbers. All are TRK-prefixed, target the track (not pad-level). The full 0–127 range is available; commonly used CCs confirmed via sweep 07:

| Param ID | Event Type | Note Values | Likely Mapping | Hardware Confirmed |
| -------- | ---------- | ----------- | -------------- | ------------------ |
| 0 | 1 | [256] | Track CC: Bank Select MSB | YES -- 2026-02-25 (sweep 07) |
| 1 | 1 | [256] | Track CC: Modulation | YES -- 2026-02-25 (sweep 07) |
| 2 | 1 | [256] | Track CC: Breath | YES -- 2026-02-25 (sweep 07) |
| 4 | 1 | [256] | Track CC: Foot | YES -- 2026-02-25 (sweep 07) |
| 5 | 1 | [256] | Track CC: Portamento | YES -- 2026-02-25 (sweep 07) |
| 6 | 1 | [256] | Track CC: Data Entry | YES -- 2026-03-01 (sweep 07) |
| 7 | 1 | [256] | Track Volume (MIDI CC7) | YES -- 2026-02-25 (Submix recording) |
| 8 | 1 | [256] | Track CC: Balance | YES -- 2026-03-01 (sweep 07) |
| 9 | 1 | [256] | Track CC: CC9 | YES -- 2026-03-01 (sweep 07) |
| 10 | 1 | [256] | Track CC: Pan | YES -- 2026-02-25 (sweep 07) |
| 11 | 1 | [256] | Track CC: Expression | YES -- 2026-02-25 (sweep 07) |
| 12 | 1 | [256] | Track CC: Effect 1 | YES -- 2026-03-01 (sweep 07) |
| 13 | 1 | [256] | Track CC: Effect 2 | YES -- 2026-03-01 (sweep 07) |
| 14 | 1 | [256] | Track CC: CC14 | YES -- 2026-03-01 (sweep 07) |
| 15 | 1 | [256] | Track CC: CC15 | YES -- 2026-03-01 (sweep 07) |
| 16 | 1 | [256] | Track CC: General Purpose 1 | YES -- 2026-03-01 (sweep 07) |

> **Note:** The full 0–127 CC range is available for automation. Only commonly used/tested CCs are listed above. Params 3 and 14 appeared in the original corpus with incorrect inferred names (track_mute and track_volume); hardware testing corrected these to their actual MIDI CC mappings.

### Track-Level Non-CC Automation (params 256–268)

| Param ID | Event Type | Note Values | Mapping | Hardware Confirmed |
| -------- | ---------- | ----------- | ------- | ------------------ |
| 256 | 1 | [256] | Track Audio Mute | YES -- 2026-02-25 (Submix recording) |
| 257 | 1 | [256] | Track Solo | YES -- 2026-02-25 (sweep 09) |
| 258 | 1 | [256] | Track Velocity | YES -- 2026-02-25 (sweep 09) |
| 259 | 1 | [256] | Track Velocity (duplicate — see notes) | YES -- 2026-03-01 (sweep 09) |
| 260 | 1 | [256] | Track: Start Clip Play | YES -- 2026-02-25 (sweep 09) |
| 261 | 1 | [256] | Track: Stop Clip Play | YES -- 2026-02-25 (sweep 09) |
| 262 | 1 | [256] | Track: Start Clip Record | YES -- 2026-02-25 (sweep 09) |
| 263 | 1 | [256] | Track: Stop Clip Record | YES -- 2026-02-25 (sweep 09) |
| 264 | 1 | [256] | Track: Start Clip Overdub | YES -- 2026-02-25 (sweep 09) |
| 265 | 1 | [256] | Track: Stop Clip Overdub | YES -- 2026-02-25 (sweep 09) |
| 266 | 1 | [256] | Track: Start Clip Overdub or Play | YES -- 2026-02-25 (sweep 09) |
| 267 | 1 | [256] | Track: Start Clip Overdub or Play (Pending Stop) | YES -- 2026-02-25 (sweep 09) |
| 268 | 1 | [256] | Track: Start Clip Play From Position | YES -- 2026-02-25 (sweep 09) |

> **Note:** Param 259 displays as "TRK: Velocity" in the MPC automation editor, identical to param 258. It may be a secondary velocity parameter or a UI quirk. Params 269–272 were also tested and display as unnamed "TRK: Parameter NNN" — possibly reserved for future clip transport features.

### Core Pad-Level Parameters (params 512–530)

All type 2, note=pad MIDI note. Pad-level automation (params 513–553) is only available on drum program tracks — keygroup and MIDI tracks do not expose these parameters (confirmed sweep 05).

| Param ID | Mapping | Hardware Confirmed |
| -------- | ------- | ------------------ |
| 512 | TRK (purpose unknown, TRK-prefixed) | YES -- 2026-02-25 (sweep 06, A01) |
| 513 | Pad Tuning | YES -- 2026-02-25 (sweep 06, A02) |
| 514 | Pad Filter Cutoff | YES -- 2026-02-25 (sweep 06, A03) |
| 515 | Pad Filter Resonance | YES -- 2026-02-25 (sweep 06, A04) |
| 516 | Pad Filter Env Depth | YES -- 2026-02-25 (sweep 06, A05) |
| 517 | Pad Pan | YES -- 2026-02-25 (sweep 06, A06) |
| 518 | Pad Level | YES -- 2026-02-25 (sweep 06, A07) |
| 519 | Pad Amp Env Attack | YES -- 2026-02-25 (sweep 06, A08) |
| 520 | Pad Amp Env Decay | YES -- 2026-02-25 (sweep 02, A01) |
| 521 | Pad Amp Env Release | YES -- 2026-02-25 (sweep 02, A02) |
| 522 | Pad Send 1 | YES -- 2026-02-25 (sweep 02, A03) |
| 523 | Pad Send 2 | YES -- 2026-02-25 (sweep 02, A04) |
| 524 | Pad Send 3 | YES -- 2026-02-25 (sweep 02, A05) |
| 525 | Pad Send 4 | YES -- 2026-02-25 (sweep 02, A06) |
| 526 | Pad Mute | YES -- 2026-02-25 (sweep 02, A07) |
| 527 | Pad Solo | YES -- 2026-02-25 (sweep 02, A08) |
| 528 | Pad Amp Env Hold | YES -- 2026-02-25 (sweep 02, A09) |
| 529 | Pad Amp Env Sustain | YES -- 2026-02-25 (sweep 02, A10) |
| 530 | Pad Amp Env Decay Mode | YES -- 2026-02-25 (sweep 02, A11) |

### Filter Envelope (params 531–536)

All type 2, note=pad MIDI note.

| Param ID | Mapping | Hardware Confirmed |
| -------- | ------- | ------------------ |
| 531 | Pad Filter Env Attack | YES -- 2026-02-25 (sweep 08, A01) |
| 532 | Pad Filter Env Hold | YES -- 2026-02-25 (sweep 08, A02) |
| 533 | Pad Filter Env Decay | YES -- 2026-02-25 (sweep 08, A03) |
| 534 | Pad Filter Env Sustain | YES -- 2026-02-25 (sweep 08, A04) |
| 535 | Pad Filter Env Release | YES -- 2026-02-25 (sweep 08, A05) |
| 536 | Pad Filter Decay Mode | YES -- 2026-02-25 (sweep 08, A06) |

### Envelope Curves (params 537–542)

All type 2, note=pad MIDI note.

| Param ID | Mapping | Hardware Confirmed |
| -------- | ------- | ------------------ |
| 537 | Pad Amp Env Attack Curve | YES -- 2026-02-25 (sweep 08, A07) |
| 538 | Pad Amp Env Decay Curve | YES -- 2026-02-25 (sweep 08, A08) |
| 539 | Pad Amp Env Release Curve | YES -- 2026-02-25 (sweep 08, A09) |
| 540 | Pad Filter Env Attack Curve | YES -- 2026-02-25 (sweep 08, A10) |
| 541 | Pad Filter Env Decay Curve | YES -- 2026-02-25 (sweep 08, A11) |
| 542 | Pad Filter Env Release Curve | YES -- 2026-02-25 (sweep 08, A12) |

### Pitch Envelope (params 543–553)

All type 2, note=pad MIDI note.

| Param ID | Mapping | Hardware Confirmed |
| -------- | ------- | ------------------ |
| 543 | Pad Pitch Env Attack | YES -- 2026-02-25 (sweep 08, A13) |
| 544 | Pad Pitch Env Hold | YES -- 2026-02-25 (sweep 08, A14) |
| 545 | Pad Pitch Env Decay | YES -- 2026-02-25 (sweep 08, A15) |
| 546 | Pad Pitch Env Sustain | YES -- 2026-02-25 (sweep 08, A16) |
| 547 | Pad Pitch Env Release | YES -- 2026-02-25 (sweep 08, A01 round 2) |
| 548 | Pad Pitch Env Decay Mode | YES -- 2026-02-25 (sweep 08, A02 round 2) |
| 549 | Pad Pitch Env Attack Curve | YES -- 2026-02-25 (sweep 08, A03 round 2) |
| 550 | Pad Pitch Env Decay Curve | YES -- 2026-02-25 (sweep 08, A04 round 2) |
| 551 | Pad Pitch Env Release Curve | YES -- 2026-02-25 (sweep 10, A01) |
| 552 | Pad Pitch Env Depth | YES -- 2026-02-25 (sweep 10, A02) |
| 553 | Pad Pitch Env Type | YES -- 2026-02-25 (sweep 10, A03) |

> **Note:** Params 554+ enter TRK: Keygroup Legacy Mode territory — end of the pad param block (confirmed sweep 10). The pad param block is 513–553 (41 named params).

### Pad Layer Automation (params 1039–1080)

Pad layer parameters follow an 8-property layout per layer. Layer N base = 1037 + (N-1) * 8.
Properties per layer: Sample, Root Note, Level, Sample Pan, Vel Start, Vel End, Semi Tune, Fine Tune.
MPC only shows layer params when that layer is populated. Layer 1 starts at 1037 (Sample) and 1038 (Root Note), inferred from the 8-property pattern.

| Param ID | Mapping | Hardware Confirmed |
| -------- | ------- | ------------------ |
| 1039 | Pad Layer 1: Level | YES -- 2026-02-25 (sweep 03, A01) |
| 1040 | Pad Layer 1: Sample Pan | YES -- 2026-02-25 (sweep 03, A02) |
| 1041 | Pad Layer 1: Vel Start | YES -- 2026-02-25 (sweep 03, A03) |
| 1042 | Pad Layer 1: Vel End | YES -- 2026-02-25 (sweep 03, A04) |
| 1043 | Pad Layer 1: Semi Tune | YES -- 2026-02-25 (sweep 03, A05) |
| 1044 | Pad Layer 1: Fine Tune | YES -- 2026-02-25 (sweep 03, A06) |
| 1045 | Pad Layer 2: Sample | YES -- 2026-02-25 (sweep 03, A07) |
| 1046 | Pad Layer 2: Root Note | YES -- 2026-02-25 (sweep 03, A08) |
| 1047 | Pad Layer 2: Level | YES -- 2026-02-25 (sweep 03, A09) |
| 1048 | Pad Layer 2: Sample Pan | YES -- 2026-02-25 (sweep 03, A10) |
| 1049 | Pad Layer 2: Vel Start | INFERRED from pattern (Layer 2) |
| 1050 | Pad Layer 2: Vel End | INFERRED from pattern (Layer 2) |
| 1051 | Pad Layer 2: Semi Tune | INFERRED from pattern (Layer 2) |
| 1052 | Pad Layer 2: Fine Tune | INFERRED from pattern (Layer 2) |
| 1053 | Pad Layer 3: Sample | YES -- 2026-02-25 (sweep 03, A15) |
| 1054 | Pad Layer 3: Root Note | YES -- 2026-02-25 (sweep 03, A16) |
| 1055 | Pad Layer 3: Level | YES -- 2026-02-25 (sweep 03, A01 round 2) |
| 1056 | Pad Layer 3: Sample Pan | YES -- 2026-02-25 (sweep 03, A02 round 2) |
| 1057 | Pad Layer 3: Vel Start | YES -- 2026-02-25 (sweep 03, A03 round 2) |
| 1058 | Pad Layer 3: Vel End | YES -- 2026-02-25 (sweep 03, A04 round 2) |
| 1059 | Pad Layer 3: Semi Tune | YES -- 2026-02-25 (sweep 03, A05 round 2) |
| 1060 | Pad Layer 3: Fine Tune | YES -- 2026-02-25 (sweep 03, A06 round 2) |
| 1061 | Pad Layer 4: Sample | YES -- 2026-02-25 (sweep 03, A07 round 2) |
| 1062 | Pad Layer 4: Root Note | YES -- 2026-02-25 (sweep 03, A08 round 2) |
| 1063 | Pad Layer 4: Level | YES -- 2026-02-25 (sweep 03, A09 round 2) |
| 1064 | Pad Layer 4: Sample Pan | YES -- 2026-02-25 (sweep 03, A10 round 2) |
| 1065 | Pad Layer 4: Vel Start | YES -- 2026-02-25 (sweep 03, A11 round 2) |
| 1066 | Pad Layer 4: Vel End | YES -- 2026-02-25 (sweep 03, A12 round 2) |
| 1067 | Pad Layer 4: Semi Tune | YES -- 2026-02-25 (sweep 03, A13 round 2) |
| 1068 | Pad Layer 4: Fine Tune | YES -- 2026-02-25 (sweep 03, A14 round 2) |
| 1077 | Pad Vel to Start | YES -- 2026-02-25 (sweep 03, A07 round 3) |
| 1078 | Pad Filter Keytrack | YES -- 2026-02-25 (sweep 03, A08 round 3) |
| 1079 | Pad Vel to Filter Attack | YES -- 2026-02-25 (sweep 03, A09 round 3) |
| 1080 | Pad Vel to Filter Depth | YES -- 2026-02-25 (sweep 03, A10 round 3) |

> **Note:** Layers 5–8 follow the same 8-property pattern (params 1069–1076 and beyond). When unpopulated, the MPC displays these as "PAD:Parameter NNNN" with generic names. The MPC dynamically shows layer automation only when that layer has content (e.g., adding a sample to Layer 5 reveals all 8 Layer 5 params). Whether params 1077–1080 are Layer 6 params with alternative names or separate pad-level params remains an open question — testing with 6+ layers populated is needed.

### Plugin Automation (params 4104+)

Plugin parameter slot = `param_id - 4096`. Without an actual plugin loaded, the MPC shows generic slot names (e.g., "TRK:Plugin Parameter 9"). With a loaded plugin, parameter names reflect the plugin controls (e.g., "Cutoff", "Resonance").

| Param ID | Event Type | Note Values | Mapping | Hardware Confirmed |
| -------- | ---------- | ----------- | ------- | ------------------ |
| 4104 | 2 | [256] | Plugin Parameter slot 8 (first seen in corpus) | INFERRED (plugin track only) |
| 4105–4120 | 2 | [256] | Plugin Parameter slots 9–24 | YES -- 2026-03-01 (sweep 04, generic names) |

## Probed But No Effect

Parameter IDs tested during hardware sweep that produced no observable UI change or showed only unnamed/generic display. These narrow the unknown parameter space.

| Param ID | Sweep Range | Firmware | Date Tested | Notes |
| -------- | ----------- | -------- | ----------- | ----- |
| 269–272 | 256–272 | 3.7.0.56 | 2026-03-01 | Display as unnamed "TRK: Parameter NNN" — possibly reserved |
| 554–570 | 551–570 | 3.7.0.56 | 2026-03-01 | All show "TRK: Keygroup Legacy Mode" — end of pad param block |
| 1069–1076 | 1039–1080 | 3.7.0.56 | 2026-03-01 | Display as "PAD:Parameter NNNN" — Layer 5 slots, hidden when unpopulated |

## Parameter Address Space Layout

Confirmed through sweep experiments 01–14:

| Range | Category | Event Type | Count | Status |
| ----- | -------- | ---------- | ----- | ------ |
| 0–127 | MIDI CC automation (track-level, TRK: prefix) | 1 | 128 | Confirmed (sweep 07) |
| 128–255 | Unknown (likely more track-level) | — | — | Untested |
| 256–268 | Track mute/solo/velocity + clip transport | 1 | 13 | Confirmed (sweep 09) |
| 269–511 | Unknown track-level range | — | — | 269–272 tested, unnamed |
| 512 | TRK: Parameter 512 (boundary/unknown) | 2 | 1 | Confirmed (sweep 06) |
| 513–553 | Pad-level automation (drum programs only) | 2 | 41 | Confirmed (sweeps 02, 06, 08, 10) |
| 554–1036 | Keygroup Legacy Mode / gap | — | — | 554–570 tested, all legacy mode |
| 1037–1100 | Layer-level automation (8 params x 8 layers) | 2 | ~64 | Layers 1–4 confirmed (sweep 03) |
| 4096+ | Plugin parameter slots (slot = param - 4096) | 2 | open | 4104–4120 confirmed (sweep 04) |

## Automation Availability by Track Type

Pad-level automation parameters are drum-program-specific. Confirmed via sweep 05 (keygroup/MIDI comparison):

| Param Range | Drum | Keygroup | MIDI |
| ----------- | ---- | -------- | ---- |
| 0–127 (MIDI CCs) | Yes | Yes (likely) | Yes (likely) |
| 256–268 (track transport) | Yes | Yes (likely) | Yes (likely) |
| 513–553 (pad params) | **Yes** | **No** | **No** |
| 1037–1100 (layer params) | Yes | Unknown | N/A |
| 4096+ (plugin params) | Yes (generic) | Unknown | N/A |

## Track-Level vs Pad-Level

| | Type 1 (Track) | Type 2 (Pad) |
| - | -------------- | ------------ |
| Event type | 1 | 2 |
| `note` field | 256 (sentinel) or pad MIDI note | Pad MIDI note or 256 |
| Scope | Entire track | Specific pad |
| Params observed | 0–127 (MIDI CC), 256–268 | 512–553 (pad params), 1039–1080 (layer params), 4104+ (plugin) |

### The note=256 Sentinel

When `automation.note = 256`, the automation targets the track as a whole (not a specific pad). MIDI notes 0–127 target specific pads.

**Exception:** Parameter 131 uses type 1 events but has note values 0–82 (pad MIDI notes), suggesting it automates pad-specific parameters via track-level events. See the Parameter 131 anomaly section below.

## Parameter 131 — Anomalous

> **ATTENTION:** Parameter 131 dominates all automation data in the corpus. Its behavior is structurally anomalous and its purpose is unknown. Downstream implementors must handle it specially.

### Anomalous Volume

| Metric | Value |
| ------ | ----- |
| Total events with param 131 | **9,432** across 11 files |
| Percentage of all automation | **95%** (9,432 of 9,927 total) |
| Tracks involved | 42 distinct tracks |
| Files involved | 11 of 18 (including multiple Akai factory demo imports) |

### Anomalous Structure

Parameter 131 events are **type 1** (track-level) but carry **pad MIDI note values** in their `automation.note` field — not the expected `note=256` track-level sentinel.

| Expected behavior (type 1) | Actual behavior (param 131) |
| -------------------------- | --------------------------- |
| `automation.note = 256` | `automation.note = 0–82` (21 unique note values) |
| Targets entire track | Note values match pad MIDI notes |
| Track-level parameter | Appears to target specific pads |

### Hypotheses

The combination of type-1 events + pad MIDI note values + normalized values 0.0–1.0 suggests one of:

1. **Pad trigger velocity automation** — automates the velocity of pad trigger events
2. **Clip trigger data** — encodes clip-launch velocity or probability
3. **Internal MPC state** — serialized transient hardware state that should not be reproduced in new projects

The extreme volume (95% of all automation) in factory demo files suggests this may be baked-in content automation rather than user-authored data.

### Value Range

- Values span the full 0.0–1.0 range
- No clustering at 0.0 or 1.0 — values are continuously distributed
- This is consistent with velocity or level automation (not binary on/off)

## Parameter Categories (from MPC manual)

All 10 automation categories are now confirmed or substantially mapped after Phase 10 hardware testing:

| Category | Confirmed Params | Status |
| -------- | ---------------- | ------ |
| Track Volume | 7 (MIDI CC7) | Confirmed |
| Track Pan | 10 (MIDI CC10) | Confirmed |
| Track Mute | 256 | Confirmed |
| Pad Volume | 518 (pad_level) | Confirmed |
| Pad Pan | 517 (pad_pan) | Confirmed |
| Pad Filter Cutoff | 514 (pad_filter_cutoff) | Confirmed |
| Pad Filter Resonance | 515 (pad_filter_resonance) | Confirmed |
| Pad Tune | 513 (pad_tuning) | Confirmed |
| Send Levels | 522-525 (pad_send_1 through pad_send_4) | Confirmed |
| Plugin Parameters | 4104 (plugin_param_0) | Inferred — plugin track only |

> **Phase 10 outcome:** Hardware testing confirmed 80+ automation parameter IDs across all 10 manual categories. Original corpus showed 8 IDs with "Likely Mapping" guesses; all have been correctly re-mapped and the full pad parameter block (512–553), layer parameter block (1039–1068), MIDI CC block (0–127), track transport block (256–268), and plugin block (4096+) have been confirmed.

## Automation Event Density (from Original Corpus)

| Param | Files | Events | Track Names |
| ----- | ----- | ------ | ----------- |
| 131 | 11 | 9,432 | 42 tracks |
| 14 | 2 | 119 | EDM Pluck, Trap SFX, Trap Syn 1 |
| 1039 | 1 | 128 | Drum 002 |
| 526 | 1 | 94 | Drum 002 |
| 1047 | 1 | 76 | Drum 002 |
| 4104 | 1 | 65 | Plugin 001 |
| 520 | 2 | 12 | Drum 002 |
| 3 | 1 | 1 | Garage Bass |

> **Note:** Corpus IDs 3, 14, 520, 526, 1039, 1047 had "inferred" mappings pre-Phase 10. Post hardware validation: param 3 was not a track mute (mute = 256), param 14 was not track volume (volume = 7/MIDI CC7), param 520 was not pad volume (volume = 518/pad_level), param 526 was not pad pan (pan = 517/pad_pan), param 1039 is pad_layer1_level (not filter cutoff, which is 514), param 1047 is pad_layer2_level (not filter resonance, which is 515). The old inferred names were reasonable guesses but Phase 10 hardware testing gave correct mappings.

Parameter 131 dominates — it accounts for 95% of all automation events. See the Parameter 131 anomaly section above for full analysis.

## Envelope Structure Summary

Three parallel envelope groups, each following the same AHDSR + Decay Mode pattern:

| Param Range | Envelope | Params |
| ----------- | -------- | ------ |
| 519–521, 528–530 | Amp Env | Attack, Decay, Release, Hold, Sustain, Decay Mode |
| 531–536 | Filter Env | Attack, Hold, Decay, Sustain, Release, Decay Mode |
| 543–548 | Pitch Env | Attack, Hold, Decay, Sustain, Release, Decay Mode |

Curve params (537–542, 549–551) control envelope shape per attack/decay/release stage across all three envelope types.

## Notes

- All automation values are normalized 0.0–1.0 regardless of the underlying parameter range.
- Parameter IDs are structured: MIDI CC block 0–127, track block 256–268, pad param block 512–553 (41 params), layer params 1037+ (8 per layer), plugin 4104+.
- Default automation lanes added by the MPC on first save: Velocity, Probability, Ratchet, Articulation, MIDI:0:Bank Sel MSB. These appear per-pad and per-track even without user-authored automation data.
- The MPC requires `numFilterTypes: 30` in clip eventList structure for automation lanes to display correctly in the editor.
