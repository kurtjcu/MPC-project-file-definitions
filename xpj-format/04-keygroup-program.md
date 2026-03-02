# 04 — Keygroup Program

> **XPJ Format** | [Index](README.md) | **Manual:** [MPC Feature Reference](A1-mpc-feature-reference.md)

Keygroup tracks (type 1) provide chromatic multi-zone sampling. They reuse the drum infrastructure (`program.drum`) for per-pad instrument slots and add a `program.keygroup` object with zone mapping metadata.

## Schema

```json
{
  "program": {
    "type": 1,
    "drum": {
      "instruments": [ /* 128 slots, same structure as drum program */ ]
    },
    "keygroup": {
      "version": 1,
      "numKeygroups": 8,
      "keygroupVersion": 1,
      "pitchBendRange": 2,
      "pitchBendPositiveRange": 2,
      "pitchBendNegativeRange": 2,
      "wheelToLfo": 0,
      "transpose": 0,
      "timbreShift": 0.0,
      "legacyMode": 0,
      "afterTouchToFilter": 0,
      "channelPressureToFilter": 0.0,
      "CurrentStackProcessor": 0,
      "ampEnvelopeGlobal": { /* ADSR */ },
      "filterEnvelopeGlobal": { /* ADSR */ },
      "auxEnvelopeGlobal": { /* ADSR */ },
      "pitchEnvelopeGlobal": { /* ADSR */ },
      "synthSection": { /* filter, LFO */ },
      "unisonProcessor": { /* voice stacking */ },
      "harmoniserProcessor": { /* pitch harmonizer */ },
      "modlinksData": { /* modulation routing */ },
      "noteCounters": { /* note priority logic */ }
    }
  }
}
```

## How Keygroups Map to Zones

The `numKeygroups` field declares how many zones are active. Each zone corresponds to an `instruments[]` slot (index 0 through `numKeygroups - 1`), with `lowNote` and `highNote` defining the MIDI key range:

```
instruments[0]: lowNote=0,  highNote=38   → bass range
instruments[1]: lowNote=39, highNote=45   → lower mid
instruments[2]: lowNote=46, highNote=52   → mid
...
instruments[7]: lowNote=81, highNote=127  → treble range
```

The key difference from drum programs: keygroup layers have `keyTrackEnable: true`, which makes pitch follow the played MIDI note.

## Zone Configurations Observed

| Pattern | numKeygroups | Description | Examples |
| ------- | ------------ | ----------- | -------- |
| Single zone | 1 | One sample covers full range (0–127) | Garage Bass, EDM Pluck |
| Split zones | 2–8 | Non-overlapping key ranges | Strings (8), Dance Bass (2) |
| Multi-sample | 16–30 | Fine pitch mapping (3–5 note ranges) | HipHop Bass (19), Trap Syn 1 (30) |
| Polyphonic | 1 (128 active) | All 128 pads active, each covering 0–127 | DH Synth, FH Stab, TH Synth |

**Polyphonic pattern:** `numKeygroups=1` but all 128 instrument slots have loaded samples with `lowNote=0, highNote=127`. Each pad holds one pitch of a multi-sampled instrument.

## Keygroup-Specific Fields

| Field | Type | Description |
| ----- | ---- | ----------- |
| `numKeygroups` | int | Number of active zones (1–128) |
| `pitchBendRange` | int | Pitch bend range in semitones |
| `wheelToLfo` | int | Mod wheel to LFO routing depth |
| `transpose` | int | Keygroup-level transposition |
| `timbreShift` | float | Timbre morphing parameter |
| `afterTouchToFilter` | int | Aftertouch → filter cutoff |
| `ampEnvelopeGlobal` | ADSR | Master amplitude envelope |
| `filterEnvelopeGlobal` | ADSR | Master filter envelope |
| `synthSection` | object | Keygroup-level filter and LFO |
| `unisonProcessor` | object | Voice stacking / detune |
| `harmoniserProcessor` | object | Pitch harmonizer |
| `modlinksData` | object | Modulation routing table |

## Keygroup Keys (22 confirmed)

All 18 keygroup tracks share this exact key set:
`CurrentStackProcessor`, `afterTouchToFilter`, `ampEnvelopeGlobal`, `auxEnvelopeGlobal`, `channelPressureToFilter`, `filterEnvelopeGlobal`, `harmoniserProcessor`, `keygroupVersion`, `legacyMode`, `modlinksData`, `noteCounters`, `numKeygroups`, `pitchBendNegativeRange`, `pitchBendPositiveRange`, `pitchBendRange`, `pitchEnvelopeGlobal`, `synthSection`, `timbreShift`, `transpose`, `unisonProcessor`, `version`, `wheelToLfo`

## Notes

- Keygroup tracks always have BOTH `program.drum` and `program.keygroup` sub-objects.
- The `drum.instruments[]` array provides the underlying sample/synth/mixer infrastructure; `keygroup` provides the zone mapping and performance controls.
- Maximum observed keygroups: 30 (Trap Syn 1, trap-demo-import).
- The "polyphonic" pattern (128 active pads all covering 0–127) represents multi-sampled instruments where each pad holds a different pitch sample.

Full example: [instrument-keygroup.json](examples/instrument-keygroup.json)
