# 08 — Effects Catalog

> **XPJ Format** | [Index](README.md) | **Manual:** [MPC Feature Reference](A1-mpc-feature-reference.md) | 103 effects confirmed

103 unique insert effect UIDs have been confirmed across two hardware test projects and corpus analysis. Effects are stored under `program.mixable.inserts.effects[]` in the JSON XPJ format.

### Discovery History

| Date | Source File | New UIDs | Running Total |
|------|-----------|----------|---------------|
| Pre-2026-03 | Corpus analysis (17 XML project files) | 27 | 27 |
| 2026-03-02 | `effects-catalog-test.xpj` (HW test 1) | 55 | 82 |
| 2026-03-02 | `Fx-test-02.xpj` (HW test 2 — full menu sweep) | 21 | 103 |

## Confirmed Effects (103 UIDs)

### Native MPC Effects (Akai) — 58 UIDs

UIDs in the `1094xxxxxx` range. Manufacturer: `Akai Professional`.

| UID | Name | Category | Source |
| --- | ---- | -------- | ------ |
| 1094932033 | Reverb Large 2 | Reverb | HW test 1 |
| 1094932034 | Reverb Medium | Reverb | corpus (15 files, 47 instances) |
| 1094932035 | Reverb Small | Reverb | corpus (3 files, 4 instances) |
| 1094932036 | Reverb In Gate | Reverb | HW test 1 |
| 1094932037 | Reverb Out Gate | Reverb | HW test 1 |
| 1094932039 | Compressor Opto | Dynamics | HW test 1 |
| 1094932040 | Compressor VCA | Dynamics | corpus (1 file, 2 instances) |
| 1094932041 | Compressor Vintage | Dynamics | HW test 1 |
| 1094932042 | Bus Compressor | Dynamics | corpus (7 files, 10 instances) |
| 1094932043 | Distortion Fuzz | Distortion | HW test 1 |
| 1094932044 | Distortion Amp | Distortion | HW test 1 |
| 1094932045 | Distortion Overdrive | Distortion | HW test 1 |
| 1094932046 | Distortion Custom | Distortion | HW test 1 |
| 1094932047 | Distortion Grimey | Distortion | HW test 1 |
| 1094932048 | Decimator | Distortion | HW test 1 |
| 1094932049 | Transient Shaper | Dynamics | HW test 1 |
| 1094932050 | Frequency Shifter | Modulation | HW test 1 |
| 1094932051 | Resampler | Special | HW test 2 |
| 1094932052 | Mother Ducker Input | Special | corpus (1 file, 1 instance) |
| 1094932053 | Delay Mono Sync | Delay | HW test 1 |
| 1094932054 | Delay Sync | Delay | corpus (11 files, 22 instances) |
| 1094932055 | Delay Analog Sync | Delay | corpus (9 files, 16 instances) |
| 1094932056 | Delay Tape Sync | Delay | HW test 1 |
| 1094932057 | Autopan Sync | Modulation | corpus (1 file, 1 instance) |
| 1094932058 | Flanger Sync | Modulation | HW test 1 |
| 1094932066 | LP Filter | Filter | HW test 1 |
| 1094932067 | HP Filter | Filter | corpus (5 files, 5 instances) |
| 1094932068 | LP Shelving Filter | Filter | HW test 2 |
| 1094932069 | HP Shelving Filter | Filter | HW test 2 |
| 1094932070 | PEQ 4-Band | EQ | HW test 1 |
| 1094932071 | PEQ 2-Band, 2-Shelf | EQ | corpus (2 files, 2 instances) |
| 1094932072 | LP Filter Sweep | Filter | corpus (1 file, 2 instances) |
| 1094932073 | HP Filter Sweep | Filter | HW test 2 |
| 1094932074 | Delay Mono | Delay | HW test 1 |
| 1094932075 | Delay Stereo | Delay | corpus (1 file, 1 instance) |
| 1094932076 | Delay Ping Pong | Delay | corpus (1 file, 1 instance) |
| 1094932077 | Delay Analog | Delay | HW test 1 |
| 1094932078 | Delay LP | Delay | HW test 1 |
| 1094932079 | Delay HP | Delay | HW test 1 |
| 1094932080 | Delay Multi-Tap | Delay | HW test 1 |
| 1094932081 | Autopan | Modulation | HW test 1 |
| 1094932082 | Chorus 4-voice | Modulation | corpus (1 file, 1 instance) |
| 1094932083 | Chorus 2-voice | Modulation | corpus (2 files, 2 instances) |
| 1094932084 | Flanger | Modulation | corpus (1 file, 4 instances) |
| 1094932086 | Phaser 1 | Modulation | HW test 1 |
| 1094932087 | Phaser 2 | Modulation | HW test 2 |
| 1094932088 | Tremolo | Modulation | HW test 1 |
| 1094932089 | Auto Wah | Modulation | HW test 1 |
| 1094932090 | Reverb Large | Reverb | corpus (13 files, 59 instances) |
| 1094940244 | Mother Ducker | Special | corpus (1 file, 2 instances) |
| 1094940257 | LP Filter Sync | Filter | HW test 2 |
| 1094940258 | HP Filter Sync | Filter | corpus (1 file, 2 instances) |
| 1094940259 | Phaser Sync | Modulation | HW test 2 |
| 1094940260 | Tremolo Sync | Modulation | HW test 2 |
| 1094940261 | MPC60 | Special | corpus (2 files, 3 instances) |
| 1094940262 | MPC3000 | Special | HW test 2 |
| 1094940263 | SP1200 | Special | HW test 2 |
| 1094940264 | SP1200 ring | Special | HW test 2 |

### AIR Music Technology Effects — 40 UIDs

UIDs in the `1631xxxxxx` range (plus one at `1633xxxxxx`). Manufacturer: `AIR Music Technology`.

| UID | Name | Category | Source |
| --- | ---- | -------- | ------ |
| 1631807571 | AIR Channel Strip | Channel | corpus (1 file, 1 instance) |
| 1631807602 | AIR Chorus | Modulation | HW test 1 |
| 1631994195 | AIR Amp Sim | Distortion | HW test 2 |
| 1631994736 | AIR Compressor | Dynamics | HW test 1 |
| 1631994947 | AIR Diode Clip | Distortion | HW test 1 |
| 1631994948 | AIR Diff Delay | Delay | HW test 1 |
| 1631994988 | AIR Delay | Delay | corpus (5 files, 15 instances) |
| 1631994995 | AIR Distortion | Distortion | HW test 2 |
| 1631995224 | AIR Expander | Dynamics | HW test 1 |
| 1631995240 | AIR Enhancer | Special | HW test 1 |
| 1631995246 | AIR Ensemble | Modulation | HW test 1 |
| 1631995463 | AIR Filter Gate | Filter | HW test 1 |
| 1631995475 | AIR Freq Shift | Modulation | HW test 1 |
| 1631995479 | AIR Fuzz-Wah | Distortion | HW test 1 |
| 1631995500 | AIR Flanger | Modulation | HW test 1 |
| 1631995988 | AIR Half Speed | Special | HW test 2 |
| 1631996741 | AIR Kill EQ | EQ | HW test 1 |
| 1631996998 | AIR Lo-Fi | Distortion | HW test 1 |
| 1631997037 | AIR Limiter | Dynamics | corpus (5 files, 5 instances) |
| 1631997251 | AIR Multi-Chorus | Modulation | HW test 2 |
| 1631997284 | AIR Multitap Delay | Delay | HW test 1 |
| 1631997304 | AIR Maximizer | Dynamics | HW test 1 |
| 1631997511 | AIR Noise Gate | Dynamics | HW test 1 |
| 1631997522 | AIR Non-Lin Reverb | Reverb | HW test 1 |
| 1631998021 | AIR Para EQ | EQ | corpus (5 files, 5 instances) |
| 1631998035 | AIR Pitch Shifter | Modulation | HW test 2 |
| 1631998067 | AIR Phaser | Modulation | HW test 1 |
| 1631998069 | AIR Pumper | Dynamics | HW test 1 |
| 1631998582 | AIR Reverb | Reverb | corpus (5 files, 15 instances) |
| 1631998790 | AIR Filter | Filter | HW test 2 |
| 1631998807 | AIR Stereo Width | Special | HW test 1 |
| 1631998832 | AIR Spring Reverb | Reverb | HW test 1 |
| 1631998836 | AIR Stutter | Special | HW test 1 |
| 1631999042 | AIR Talk Box | Vocal | HW test 2 |
| 1631999044 | AIR Tube Drive | Distortion | corpus (5 files, 5 instances) |
| 1631999090 | AIR Transient | Dynamics | HW test 1 |
| 1631999556 | AIR Vocal Doubler | Vocal | HW test 1 |
| 1631999560 | AIR Vocal Harmonizer | Vocal | HW test 1 |
| 1631999572 | AIR Vocal Tuner | Vocal | HW test 1 |
| 1633044076 | AIR Vintage Filter | Filter | HW test 2 |

### Special / Other UIDs — 5 UIDs

UIDs outside the Akai (`1094xxxxxx`) and AIR (`1631-1633xxxxxx`) ranges:

| UID | Name | Type | Source |
| --- | ---- | ---- | ------ |
| 1229866834 | Granulator | Performance effect | HW test 1 |
| 1397572684 | Sample Delay | Utility effect | HW test 1 |
| 1414096504 | TouchFX | Touch controller effect | HW test 2 |
| 1481655884 | AIR Flavor | Special | HW test 2 |
| 1482245752 | XYFX | XY pad processor | corpus |

Note: Mother Ducker Input (1094932052) and Mother Ducker Output (1094940244) are listed in the Akai table above. They are paired effects that require both UIDs to function.

### Plugin Instruments (not effects)

| UID | Name | Notes |
| --- | ---- | ----- |
| 1114860654 | Bassline | Plugin instrument (category=1, isInstrument=true) |

## UID Families

| Range | Manufacturer | Prefix | Count |
| ----- | ------------ | ------ | ----- |
| `1094xxxxxx` | Akai Professional | (none) | 58 |
| `1631xxxxxx` | AIR Music Technology | `MPC:` | 39 |
| `1633xxxxxx` | AIR Music Technology | `MPC:` | 1 |
| `1229xxxxxx` | Unknown (Granulator) | — | 1 |
| `1397xxxxxx` | Unknown (Sample Delay) | — | 1 |
| `1414xxxxxx` | Special / TouchFX | — | 1 |
| `1481xxxxxx` | Special / AIR Flavor | — | 1 |
| `1482xxxxxx` | Special / XYFX | — | 1 |
| `1114xxxxxx` | Plugin instruments | `MPC:` | 1 |

## Plugin Description Schema

Effects are stored at `program.mixable.inserts.effects[]`. Two schema versions have been observed across different XPJ formats:

### JSON XPJ format (MPC firmware 3.x, gzip-compressed)

All hardware test files use this format. Effects live in `program.mixable.inserts.effects[i].plugin.description`:

```json
{
  "version": 1,
  "name": "Delay Analog Sync",
  "descriptiveName": "Delay Analog Sync",
  "pluginFormatName": "MPC",
  "category": "",
  "manufacturerName": "Akai Professional",
  "fileOrIdentifier": "Delay Analog Sync",
  "uid": 1631994988,
  "isInstrument": false,
  "numInputChannels": 2,
  "numOutputChannels": 2
}
```

Key differences from older format:
- `uid` (lowercase) instead of `UID`
- `manufacturerName` is populated (e.g. `"Akai Professional"`, `"AIR Music Technology"`)
- `descriptiveName` and `pluginFormatName` fields added
- `version` field present (always 1)
- `category` is an empty string (not an integer)
- No `files` array
- AIR effects use `"MPC:"` prefix in `fileOrIdentifier` (e.g. `"MPC:AIR Delay"`); Akai effects do not

### XML XPJ format (older MPC firmware, corpus files)

Older corpus projects use XML format with a different description schema:

```json
{
  "UID": 1094932090,
  "name": "Reverb Large",
  "manufacturer": "",
  "fileOrIdentifier": "Reverb Large",
  "category": 0,
  "isInstrument": false,
  "numInputChannels": 2,
  "numOutputChannels": 2,
  "files": []
}
```

Key differences from newer format:
- `UID` (uppercase) instead of `uid`
- `manufacturer` (empty string) instead of populated `manufacturerName`
- `category` is integer (0 for effects, 1 for instruments)
- `files` array present (always empty for built-in effects)
- No `version`, `descriptiveName`, or `pluginFormatName` fields

**Parser note:** Code should check for both `uid` and `UID` keys when reading effect descriptions to support both formats.

## Coverage Summary

- **Total confirmed:** 103 effect UIDs (27 from corpus, +55 from HW test 1, +21 from HW test 2)
- **Akai native:** 58 UIDs (includes Mother Ducker Input + Output pair)
- **AIR effects:** 40 UIDs (39 in `1631xxxxxx` range + 1 at `1633044076`)
- **Other:** 5 UIDs (Granulator, Sample Delay, TouchFX, AIR Flavor, XYFX)
- **Remaining gaps:** Paid expansion effects (not installed on test hardware) may add additional UIDs

## Notes

- Most common effects in corpus: Reverb Large (59 instances), Reverb Medium (47), Delay Sync (22).
- Mother Ducker requires paired Input + Output effects (two separate UIDs).
- MPC60 effect has 0 presets (all others have 1).
- Effect state is stored as opaque base64 — parameters cannot be read or written without the effect plugin.
- "Distortion" in the manual is "Distortion Custom" on hardware (UID 1094932046).
- "Distortion Grimey", "Granulator", "Resampler" were not in original test project lists — discovered during hardware testing.
- HW test 2 discovered retro sampler effects: MPC3000 (1094940262), SP1200 (1094940263), SP1200 ring (1094940264).
- AIR Vintage Filter (1633044076) is the only AIR effect outside the `1631xxxxxx` range.
- TouchFX (1414096504) and AIR Flavor (1481655884) have unique UID prefixes not shared by other effects.
