# 12 — Samples

> **XPJ Format** | [Index](README.md) | **Manual:** [MPC Feature Reference](A1-mpc-feature-reference.md)

Samples are referenced at two levels: the track-level sample pool and per-layer references within instruments.

## Track-Level Sample Pool

Each track has a `samples[]` array containing all samples used by that track:

```json
{
  "samples": [
    {
      "version": 1,
      "name": "Kick-Tape MPC-Tpe1Q1N3",
      "path": "Kick-Tape MPC-Tpe1Q1N3.wav",
      "loadImpl": 0,
      "metadata": {
        "tempo": 300.0000305175781,
        "rootNote": 60,
        "tune": 0.0,
        "key": "A# Natural Minor"
      }
    }
  ]
}
```

## Sample Fields

| Field | Type | Description |
| ----- | ---- | ----------- |
| `version` | int | Always 1 |
| `name` | string | Display name (no extension) |
| `path` | string | Filename with extension |
| `loadImpl` | int | Load implementation (always 0) |
| `metadata.tempo` | float | Original tempo in BPM |
| `metadata.rootNote` | int | Root MIDI note |
| `metadata.tune` | float | Fine tuning offset |
| `metadata.key` | string | Musical key (e.g., `"G# Natural Minor"`) |

## Sample Statistics

Across 17 files, 818 total samples:

| Metric | Value |
| ------ | ----- |
| Total samples | 818 |
| Format | All WAV |
| loadImpl | Always 0 |
| Samples with metadata | 753 |

### Tempo Distribution

| Tempo (BPM) | Count | Notes |
| ------------ | ----- | ----- |
| 300.0 | 93 | Likely default / one-shot |
| 30.0 | 108 | Likely default / one-shot |
| 120.0 | 39 | Common default |
| Various | — | Actual loop tempos |

The extreme values (30, 300) are likely defaults for non-tempo-aware samples (one-shots, drum hits).

### Root Note Distribution

| Root Note | Count | MIDI Note |
| --------- | ----- | --------- |
| 60 | 717 | C4 (default) |
| 0 | varies | C-1 |
| Others | sparse | Various |

Most samples use the default root note of 60 (C4).

### Key Distribution (top 5)

| Key | Count |
| --- | ----- |
| G# Natural Minor | 105 |
| A# Major | — |
| E Natural Minor | — |
| B Major | — |
| B Natural Minor | — |

## Layer-to-Sample Reference

Drum and keygroup instruments reference samples via `sampleName` in their layer objects:

```
track.samples[i].path = "Kick-Tape MPC-Tpe1Q1N3.wav"
                 ↕
track.program.drum.instruments[j].layersv[k].sampleName = "Kick-Tape MPC-Tpe1Q1N3.wav"
```

The `sampleName` in the layer matches the `path` field in the sample pool.

## Audio Event Sample References

Audio events (type 4) embed their sample reference directly rather than referencing the track pool:

```json
{
  "audio": {
    "sample": {
      "name": "02 Music",
      "path": "02 Music.wav",
      "metadata": { "tempo": 130.875, "rootNote": 60, "key": "E Major" }
    }
  }
}
```

## Top-Level Sample Pool

`data.samples[]` contains a project-wide sample pool — the union of all track-level sample pools. This appears to be used for project loading/management.

## Notes

- All samples are WAV files — no other formats observed.
- Sample files are stored alongside the .xpj file in the project directory.
- The `metadata.key` field uses format `"Note Quality"` (e.g., `"G# Natural Minor"`, `"C Major"`).
- Samples per track range from 0 (infrastructure tracks, plugin tracks) to 115 (large drum kits).
