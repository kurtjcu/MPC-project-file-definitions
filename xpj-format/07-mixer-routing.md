# 07 — Mixer & Routing

> **XPJ Format** | [Index](README.md) | **Manual:** [MPC Feature Reference](A1-mpc-feature-reference.md)

Every program and every drum instrument has a `mixable` object controlling volume, pan, routing, sends, and insert effects.

## Mixable Structure

```json
{
  "mixable": {
    "version": 2,
    "audioRoute": {
      "destination": 0,
      "output": "Out 1/2"
    },
    "volume": 0.7079457640647888,
    "pan": 0.5039370059967041,
    "solo": false,
    "mute": false,
    "sends": [
      { "volume": 0.0 },
      { "volume": 0.0 },
      { "volume": 0.0 },
      { "volume": 0.0 }
    ],
    "inserts": {
      "version": 1,
      "effects": [
        {
          "version": 1,
          "bypass": false,
          "plugin": {
            "version": 1,
            "description": {
              "UID": 1094932090,
              "name": "Reverb Large",
              "manufacturer": "",
              "fileOrIdentifier": "Reverb Large",
              "category": 0,
              "isInstrument": false,
              "numInputChannels": 2,
              "numOutputChannels": 2,
              "files": []
            },
            "state": "<base64>",
            "presets": []
          }
        }
      ]
    }
  }
}
```

## Field Reference

| Field | Type | Description |
| ----- | ---- | ----------- |
| `audioRoute.destination` | int | Routing target type |
| `audioRoute.output` | string | Target name (e.g., `"Out 1/2"`, `"Submix 1"`) |
| `volume` | float | 0.0–1.0 linear scale |
| `pan` | float | 0.0–1.0 (0.5 = center) |
| `solo` | bool | Solo state |
| `mute` | bool | Mute state |
| `sends[4]` | array | 4 send levels, each `{ "volume": float }` |
| `inserts.effects[]` | array | Insert effect chain (variable length) |

## Routing Destinations

| `destination` | Meaning |
| ------------- | ------- |
| 0 | Default output (typically `Out 1/2`) |
| 2 | Submix |

## Where Mixable Appears

The `mixable` object appears at multiple levels:

| Location | Controls |
| -------- | -------- |
| `program.mixable` | Track-level mix (every track type) |
| `program.drum.instruments[i].mixable` | Per-pad mix (drum and keygroup tracks) |
| Infrastructure tracks (Return, Submix, Output) | Bus-level mix |

## The `mixer` Top-Level Object

`data.mixer` contains infrastructure track references that mirror the Return, Submix, Output, and Input tracks. This appears to be a redundant representation for the mixer UI — the same data is accessible via `data.tracks[]` for types 7/8/9.

```json
{
  "mixer": {
    "returnTracks": [ /* same as type-7 tracks */ ],
    "submixTracks": [ /* same as type-8 tracks */ ],
    "outputTracks": [ /* same as type-9 tracks */ ],
    "inputTracks": [  /* type-10 Input tracks (only here, not in tracks[]) */ ]
  }
}
```

**Input tracks (type 10)** only appear in `data.mixer.inputTracks`, NOT in `data.tracks[]`.

## Insert Effect Schema

Each insert effect shares the same `plugin.description` schema as plugin instruments (see [08-effects-catalog.md](08-effects-catalog.md)):

```json
{
  "version": 1,
  "bypass": false,
  "plugin": {
    "version": 1,
    "description": {
      "UID": 1094932054,
      "name": "Delay Sync",
      "manufacturer": "",
      "fileOrIdentifier": "Delay Sync",
      "category": 0,
      "isInstrument": false
    },
    "state": "<base64>",
    "presets": []
  }
}
```

## Routing Topology

```
Content tracks (Drum/Keygroup/Plugin/Audio/MIDI)
  └→ audioRoute → Submix OR Output
                    └→ Output
                        └→ Hardware Out

Send buses:
  Content track sends[0-3] → Return 1-4
    └→ Return audioRoute → Output
```

## Mother Ducker Sidechain Buses

The Mother Ducker uses 8 **internal trigger buses** separate from the 8 submix buses. A sidechain ducking setup requires two effects: the Input (on the trigger source) and the Output (on the destination being ducked).

```
Trigger Source (pad/track)        Destination (track/submix/output)
  [Mother Ducker Input]  --Bus N-->  [Mother Ducker (Output)]
  Parameter: To Bus 1-8              Parameters: From Bus 1-8, Ratio, Knee,
                                     Attack, Release, Threshold, Gain, Auto Gain
```

| Component | UID | Location | Key Parameters |
| --------- | --- | -------- | -------------- |
| Mother Ducker Input | 1094932052 | Insert on trigger source | `To Bus 1-8` |
| Mother Ducker (Output) | 1094940244 | Insert on destination | `From Bus 1-8`, Ratio, Threshold, Knee, Attack, Release, Gain, Auto Gain |

## `data.mixer` Additional Fields

Beyond infrastructure track mirrors, `data.mixer` contains global mixer parameters:

| Field | Type | Description |
| ----- | ---- | ----------- |
| `mixer.volume` | float | Master output volume (0.0–1.0; `0.7079`=−3dB) |
| `mixer.xFaderBreakpoint` | float | Crossfader breakpoint (`0.5`) |
| `mixer.xFaderCurve` | int | Crossfader curve type (`0`=Linear) |

## Notes

- Infrastructure track counts are fixed: 4 Returns, 8 Submixes, 16 Outputs (4+8+16 = 28 infrastructure tracks).
- The `pan` default of `0.5039370059967041` appears consistently — likely a float precision artifact of the MPC's internal representation. See [00-format-reference.md](00-format-reference.md) §Known Quirks.
- Insert effects carry their state as opaque base64 blobs, just like plugin instruments.
- Send levels are always 4 slots, mapping to Return 1–4.
- **Use `data.tracks[]` as the canonical source** for infrastructure track data. `data.mixer` is a redundant mirror for the mixer UI.
